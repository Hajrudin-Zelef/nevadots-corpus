---
id: collect-261121-veille-ia/go-json/json-and-request-validation-in-go-encoding-json-json-v2-struct-tags-and-body-limits-mert-ozen
title: "JSON and Request Validation in Go: encoding/json, json/v2, Struct Tags and Body Limits — Mert Özen"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["json", "items", "customerid", "encoding", "struct", "post", "field", "orders"]
source: https://www.mertozen.com/en/blog/go-json-encoding-json-v2-struct-tag-validation
source_anchor: ""
source_lines: [1, 433]
section: "Go_JSON"
---
# JSON and Request Validation in Go: encoding/json, json/v2, Struct Tags and Body Limits

Reading a JSON body safely in Go takes a Content-Type check, a size limit with MaxBytesReader, DisallowUnknownFields and a single-value check, while encoding/json v1 matches field names regardless of case and takes the last of a repeated field. POST /api/orders in Bookstore, missing values with *int, a validation list from an errors.Join tree, Problem Details with the same contract as ASP.NET Core, Go 1.27's json/v2 differences, and carrying prices in kuruş instead of float64.

Version note: The code in this post was built and run with Go 1.27.4; the module's `go` line is 1.26.0 and the application code also compiles with Go 1.26.11. The API uses `encoding/json` (v1). `encoding/json/v2` and `encoding/json/jsontext` joined the standard library in Go 1.27; the lab that shows them is set apart with `//go:build go1.27`. `omitzero` arrived in Go 1.24.

## Guess First

In this post the API of Bookstore, the online bookstore used as the shared example across this site's series, takes orders for the first time. A client sends this body to `POST /api/orders`; the same field appears twice, the second time in capitals:

```
curl -i -X POST -H 'Content-Type: application/json' \
  --data '{"customerId":"ada","CUSTOMERID":"mallory","items":[{"isbn":"9780441172719","quantity":1}]}' \
  http://127.0.0.1:52710/api/orders
```
The request struct has a single field tagged `json:"customerId"`, and we told the decoder to reject unknown fields. Is the answer a 400, or is the order written to Ada, or to Mallory?

```
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/orders/ORD-1001
X-Request-Id: guess
Content-Length: 197
{"id":"ORD-1001","customerId":"mallory","status":"Pending","items":[{"isbn":"9780441172719","title":"Dune","unitPrice":34990,"quantity":1}],"total":34990,"createdAt":"2026-11-15T01:55:12.329694Z"}
```
The order went to Mallory. `encoding/json` matches field names without regard to case, so `CUSTOMERID` was not an unknown field, and when the same field came twice the last one won. If a proxy or a WAF looks at the first value and the Go service at the last, that difference can become a security hole. `encoding/json/v2` in Go 1.27 handles both differently. In this post we look at reading and writing JSON, request validation, returning errors as Problem Details, and finally `POST /api/orders`.

## Struct Tags and DTOs

The HTTP layer does not turn domain types straight into JSON; there are separate DTOs for requests and responses, just as in the ASP.NET Core Web API series' "DTO ve Mapping" post. These types live in `internal/httpapi`, the layer where the DTOs of `Bookstore.Api` live on the C# side. In Go they are tied to JSON names with struct tags:

```
// placeOrderRequest is the body of POST /api/orders.
type placeOrderRequest struct {
    CustomerID string             `json:"customerId"`
    Items      []orderItemRequest `json:"items"`
}
```
```
type orderItemRequest struct {
    ISBN     string `json:"isbn"`
    Quantity *int   `json:"quantity"` // nil: the field was missing, not 0
}
```
```
// orderResponse is what the API returns for an order.
type orderResponse struct {
    ID         string              `json:"id"`
    CustomerID string              `json:"customerId"`
    Status     string              `json:"status"`
    Items      []orderItemResponse `json:"items"`
    Total      catalog.Money       `json:"total"` // in kuruş, like every price
    CreatedAt  time.Time           `json:"createdAt"`
}
```
```
func newOrderResponse(o *orders.Order) orderResponse {
    items := make([]orderItemResponse, 0, len(o.Items)) // never nil: [] not null
    for _, it := range o.Items {
        items = append(items, orderItemResponse{ISBN: it.ISBN, Title: it.Title, UnitPrice: it.UnitPrice, Quantity: it.Quantity})
    }
    return orderResponse{ID: o.ID, CustomerID: o.CustomerID, Status: o.Status.String(),
        Items: items, Total: o.Total(), CreatedAt: o.CreatedAt.UTC()}
}
```
`Quantity *int` is a deliberate choice. With an `int` we would see `0` even when `quantity` is missing from the body, and lose the difference between "no quantity given" and "quantity 0". In a pointer field a missing value is `nil`; it is the counterpart of C#'s `int?`. On the response side we do the opposite: `items` is always a slice, even an empty one; as we saw in post 03, a nil slice is written as `null` in v1, and `order.items.length` fails on the Angular side.

`omitempty` in a tag leaves out empty values (0, `""`, nil, an empty slice), but it has no effect on structs: a zero `time.Time` still goes out as `"0001-01-01T00:00:00Z"`. `omitzero` from Go 1.24 fixes that; the field is left out if it is the type's zero value, or if the type's `IsZero()` method returns `true`. The last line of the json/v2 lab below shows the difference.

## Reading the Body Safely: Decoder, DisallowUnknownFields, MaxBytesReader

The function that reads the body checks four things: the `Content-Type`, a size limit, unknown fields, and that the body holds a single JSON value.

```
// errUnsupportedMediaType is returned when the body is not JSON.
var errUnsupportedMediaType = errors.New("Content-Type must be application/json")
// decodeJSON reads exactly one JSON object from a body of at most maxBodyBytes
// into dst and rejects fields that dst does not have.
func decodeJSON(w http.ResponseWriter, r *http.Request, dst any) error {
    if mt, _, err := mime.ParseMediaType(r.Header.Get("Content-Type")); err != nil || mt != "application/json" {
        return errUnsupportedMediaType
    }
    r.Body = http.MaxBytesReader(w, r.Body, maxBodyBytes)
    dec := json.NewDecoder(r.Body)
    dec.DisallowUnknownFields()
    if err := dec.Decode(dst); err != nil {
        return err
    }
    if err := dec.Decode(&struct{}{}); !errors.Is(err, io.EOF) {
        return errors.New("body must contain a single JSON object")
    }
    return nil
}
// writeDecodeError maps a decodeJSON error to a problem response.
func writeDecodeError(w http.ResponseWriter, r *http.Request, err error) {
    var tooLarge *http.MaxBytesError
    switch {
    case errors.Is(err, errUnsupportedMediaType):
        writeProblem(w, r, Problem{Status: http.StatusUnsupportedMediaType, Detail: err.Error()})
    case errors.As(err, &tooLarge):
        writeProblem(w, r, Problem{Status: http.StatusRequestEntityTooLarge,
            Detail: fmt.Sprintf("body must be at most %d bytes", tooLarge.Limit)})
    default:
        writeProblem(w, r, Problem{Status: http.StatusBadRequest, Detail: err.Error()})
    }
}
```
`http.MaxBytesReader` stops reading with a `*http.MaxBytesError` once the body passes 64 KB and tells the server to close the connection; without a limit someone could fill the memory by sending gigabytes. `DisallowUnknownFields` returns an error for misspelled or unexpected fields instead of silently ignoring them. Since `Decode` reads only the first JSON value from the stream, a second `Decode` confirms that the body has ended. Different bad requests returned these (only `detail` and the status are shown):

```
{"coupon":"BOOK10", ...}                   400  json: unknown field "coupon"
{"customerId":"ada",                       400  unexpected EOF
{"quantity":"2", ...}                      400  json: cannot unmarshal string into Go struct field placeOrderRequest.items.0.quantity of type int
{...}{"customerId":"ada"}                  400  body must contain a single JSON object
70 KB body                                 413  body must be at most 65536 bytes
Content-Type: text/plain                   415  Content-Type must be application/json
```
In ASP.NET Core this behaviour comes by default: `[ApiController]` returns a 400 on model binding errors by itself, and Kestrel has a default body limit. In Go you write all of it yourself; in return you see what happens in a single function.

## Validation: By Hand or with validator?

I split validation into two layers. The HTTP layer only looks at the shape of the request: if `quantity` is missing, it returns 400 before reaching the domain. The business rules have lived in `orders.Service.Place` since post 05: the customer ID is required and the quantity must be positive. The service collected every problem into one error with `errors.Join`; I have now added the function I asked for in the ★★★ exercise of post 05 to the `orders` package:

```
// ValidationErrors returns every ValidationError in err's tree, in order.
// It walks both single wrapping (%w) and errors.Join.
func ValidationErrors(err error) []ValidationError {
    var out []ValidationError
    var walk func(error)
    walk = func(e error) {
        switch x := e.(type) {
        case nil:
        case ValidationError:
            out = append(out, x)
        case interface{ Unwrap() []error }:
            for _, inner := range x.Unwrap() {
                walk(inner)
            }
        case interface{ Unwrap() error }:
            walk(x.Unwrap())
        }
    }
    walk(err)
    return out
}
```
The function walks the error tree through two kinds of `Unwrap`: a single error wrapped with `%w` provides `Unwrap() error`, while `errors.Join` provides `Unwrap() []error`. The handler turns this list into the `errors` shape of ASP.NET Core's `ValidationProblemDetails`:

```
// writeValidationProblem lists every invalid field, in the shape of
// ASP.NET Core's ValidationProblemDetails.
func writeValidationProblem(w http.ResponseWriter, r *http.Request, verrs []orders.ValidationError) {
    fields := make(map[string][]string, len(verrs))
    for _, v := range verrs {
        fields[v.Field] = append(fields[v.Field], v.Message)
    }
    writeProblem(w, r, Problem{Status: http.StatusBadRequest,
        Title: "One or more validation errors occurred.", Errors: fields})
}
```
```
{"customerId":"","items":[{"isbn":"9780441172719","quantity":0},{"isbn":"9780141439587"}]}
{"customerId":"","items":[{"isbn":"9780441172719","quantity":0}]}
```
```
{"type":"https://tools.ietf.org/html/rfc9110#section-15.5.1","title":"One or more validation errors occurred.","status":400,"instance":"/api/orders","errors":{"items[1].quantity":["is required"]},"traceId":"missing"}
{"type":"https://tools.ietf.org/html/rfc9110#section-15.5.1","title":"One or more validation errors occurred.","status":400,"instance":"/api/orders","errors":{"customerId":["is required"],"items[0].quantity":["must be positive"]},"traceId":"invalid"}
```
The first request had a shape error, so the domain check never ran; for the second request both of the service's errors were listed together. That is the price of the two-stage design: the client sees some errors only on the second attempt. The alternative is a library such as `go-playground/validator` (v10) that writes rules into struct tags: tags like `validate:"required,min=1"` look very much like the Data Annotations from the ASP.NET Core Web API series' "Model Validation" post. It pays off as the number of rules grows, but it tempts you to move business rules into the HTTP DTO. In Bookstore the rules are few and belong to the domain, so I stayed with hand-written validation.

## Error Responses with Problem Details (RFC 9457)

Up to post 09 errors went out as plain text through `http.Error`. RFC 9457, published in July 2023, replaced RFC 7807 and defines a standard error format for HTTP APIs: the `application/problem+json` media type and the members `type`, `title`, `status`, `detail` and `instance`, plus extension members where needed. In the ASP.NET Core Web API series' "Global Hata Yönetimi" post we saw how .NET produces this format. So that Bookstore's Angular client sees the same contract from both the .NET and the Go API, I kept the type URIs and extension member names the same as in ASP.NET Core:

```
// Problem is an RFC 9457 problem details body. Errors and TraceID are
// extension members, named as ASP.NET Core names them.
type Problem struct {
    Type     string              `json:"type"`
    Title    string              `json:"title"`
    Status   int                 `json:"status"`
    Detail   string              `json:"detail,omitempty"`
    Instance string              `json:"instance,omitempty"`
    Errors   map[string][]string `json:"errors,omitempty"`
    TraceID  string              `json:"traceId,omitempty"`
}
```
```
// problemTypes uses the same type URIs as ASP.NET Core, so the Angular
// client sees one contract from both APIs.
var problemTypes = map[int]string{
    http.StatusBadRequest:            "https://tools.ietf.org/html/rfc9110#section-15.5.1",
    http.StatusNotFound:              "https://tools.ietf.org/html/rfc9110#section-15.5.5",
    http.StatusConflict:              "https://tools.ietf.org/html/rfc9110#section-15.5.10",
    http.StatusRequestEntityTooLarge: "https://tools.ietf.org/html/rfc9110#section-15.5.14",
    http.StatusUnsupportedMediaType:  "https://tools.ietf.org/html/rfc9110#section-15.5.16",
    http.StatusUnprocessableEntity:   "https://tools.ietf.org/html/rfc4918#section-11.2",
    http.StatusInternalServerError:   "https://tools.ietf.org/html/rfc9110#section-15.6.1",
}
```
```
// writeProblem fills the common members and writes p as application/problem+json.
func writeProblem(w http.ResponseWriter, r *http.Request, p Problem) {
    if p.Type == "" {
        p.Type = problemTypes[p.Status]
    }
    if p.Type == "" {
        p.Type = "about:blank"
    }
    if p.Title == "" {
        p.Title = http.StatusText(p.Status)
    }
    p.Instance = r.URL.Path
    p.TraceID = RequestIDFrom(r.Context())
    w.Header().Set("Content-Type", "application/problem+json")
    w.WriteHeader(p.Status)
    if err := json.NewEncoder(w).Encode(p); err != nil {
        log.Printf("write problem: %v", err)
    }
}
```
I put the request ID from post 09 into `traceId`; it is the only value needed to find a reported error in the log. When the error is unknown, `detail` stays empty and the details go only to the log. Go's `http.StatusText` differs from ASP.NET Core in some titles: for 413 Go says "Request Entity Too Large", ASP.NET Core "Content Too Large". A client should look at `status` and `type`, not at `title`.

## POST /api/orders: The First Order over HTTP

The handler puts all the pieces together: it reads the body, checks its shape, calls `Place` with the request's context and turns the error into a status code by its kind. On success it returns 201 and the address of the new resource in `Location`.

```
func (a *API) placeOrder(w http.ResponseWriter, r *http.Request) {
    var req placeOrderRequest
    if err := decodeJSON(w, r, &req); err != nil {
        writeDecodeError(w, r, err)
        return
    }
    lines := make([]orders.Line, 0, len(req.Items))
    var missing []orders.ValidationError
    for i, it := range req.Items {
        if it.Quantity == nil {
            missing = append(missing, orders.ValidationError{Field: fmt.Sprintf("items[%d].quantity", i), Message: "is required"})
            continue
        }
        lines = append(lines, orders.Line{ISBN: it.ISBN, Quantity: *it.Quantity})
    }
    if len(missing) > 0 {
        writeValidationProblem(w, r, missing)
        return
    }
    o, err := a.Orders.Place(r.Context(), req.CustomerID, lines)
    if verrs := orders.ValidationErrors(err); len(verrs) > 0 {
        writeValidationProblem(w, r, verrs)
        return
    }
    switch {
    case errors.Is(err, catalog.ErrBookNotFound):
        writeProblem(w, r, Problem{Status: http.StatusUnprocessableEntity, Detail: err.Error()})
        return
    case errors.Is(err, orders.ErrOutOfStock):
        writeProblem(w, r, Problem{Status: http.StatusConflict, Detail: err.Error()})
        return
    case err != nil:
        log.Printf("place order: %v rid=%s", err, RequestIDFrom(r.Context()))
        writeProblem(w, r, Problem{Status: http.StatusInternalServerError})
        return
    }
    w.Header().Set("Location", "/api/orders/"+o.ID)
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusCreated)
    if err := json.NewEncoder(w).Encode(newOrderResponse(o)); err != nil {
        log.Printf("write json: %v", err)
    }
}
```
```
curl -i -X POST -H 'Content-Type: application/json' \
  --data '{"customerId":"grace","items":[{"isbn":"9780441172719","quantity":2},{"isbn":"9780141439587","quantity":1}]}' \
  http://127.0.0.1:52710/api/orders
curl http://127.0.0.1:52710/api/orders/ORD-1002
```
```
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/orders/ORD-1002
X-Request-Id: happy
Content-Length: 265
{"id":"ORD-1002","customerId":"grace","status":"Pending","items":[{"isbn":"9780441172719","title":"Dune","unitPrice":34990,"quantity":2},{"isbn":"9780141439587","title":"Emma","unitPrice":19900,"quantity":1}],"total":89880,"createdAt":"2026-11-15T01:55:12.34426Z"}
{"id":"ORD-1002","customerId":"grace","status":"Pending","items":[{"isbn":"9780441172719","title":"Dune","unitPrice":34990,"quantity":2},{"isbn":"9780141439587","title":"Emma","unitPrice":19900,"quantity":1}],"total":89880,"createdAt":"2026-11-15T01:55:12.34426Z"}
```
A book that is out of stock gets a 409, and an ISBN that is not in the catalog gets a 422:

```
{"type":"https://tools.ietf.org/html/rfc9110#section-15.5.10","title":"Conflict","status":409,"detail":"place order: \"The Hobbit\" has 0 in stock, 1 requested: out of stock","instance":"/api/orders","traceId":"stock"}
{"type":"https://tools.ietf.org/html/rfc4918#section-11.2","title":"Unprocessable Entity","status":422,"detail":"place order: isbn 9780000000000: book not found","instance":"/api/orders","traceId":"isbn"}
```
In `main` the handlers need one line: the `API` gets the service built with `orders.NewService(memstore.New(), books)`. Since we made the service and memstore safe for concurrent use in post 08, the server handling every request in its own goroutine is no longer a problem. The `CrossOriginProtection` from post 09 has also started protecting this first POST endpoint.

## encoding/json/v2 and jsontext

Go 1.27 added two packages to the standard library: `encoding/json/v2` and `encoding/json/jsontext`, which works underneath it and handles JSON at the token and value level. According to the release notes v1 now runs on top of v2; its behaviour is preserved, but the text of error messages may change and unmarshalling became significantly faster. If something goes wrong, `GOEXPERIMENT=nojsonv2` switches back to the old implementation. I put the differences side by side in a lab:

```
//go:build go1.27
// Lab 10: encoding/json (v1) and encoding/json/v2 side by side.
package main
import (
    jsonv1 "encoding/json"
    "encoding/json/jsontext"
    "encoding/json/v2"
    "fmt"
    "time"
)
// shipment shows omitempty and omitzero (Go 1.24) on a time.Time.
type shipment struct {
    ShippedAt   time.Time `json:"shippedAt,omitempty"`  // a struct is never "empty"
    DeliveredAt time.Time `json:"deliveredAt,omitzero"` // the zero time is left out
}
type request struct {
    CustomerID string   `json:"customerId"`
    Coupons    []string `json:"coupons"`
}
func main() {
    // 1) A nil slice.
    empty := request{CustomerID: "ada"}
    b1, _ := jsonv1.Marshal(empty)
    b2, _ := json.Marshal(empty)
    b3, _ := json.Marshal(empty, json.FormatNilSliceAsNull(true))
    fmt.Printf("v1:      %s\nv2:      %s\nv2+null: %s\n", b1, b2, b3)
    // 2) The same key twice, in different case.
    body := []byte(`{"customerId":"ada","CUSTOMERID":"mallory"}`)
    var r1, r2 request
    err1 := jsonv1.Unmarshal(body, &r1)
    err2 := json.Unmarshal(body, &r2)
    fmt.Printf("v1: %q err=%v\nv2: %q err=%v\n", r1.CustomerID, err1, r2.CustomerID, err2)
    // 3) The exact same key twice.
    dup := []byte(`{"customerId":"ada","customerId":"mallory"}`)
    var d1, d2, d3 request
    err1 = jsonv1.Unmarshal(dup, &d1)
    err2 = json.Unmarshal(dup, &d2)
    err3 := json.Unmarshal(dup, &d3, jsontext.AllowDuplicateNames(true))
    fmt.Printf("v1: %q err=%v\nv2: %q err=%v\nv2+allow: %q err=%v\n", d1.CustomerID, err1, d2.CustomerID, err2, d3.CustomerID, err3)
    // 4) Invalid UTF-8 inside a string.
    bad := []byte("{\"customerId\":\"ad\xffa\"}")
    var u1, u2 request
    err1 = jsonv1.Unmarshal(bad, &u1)
    err2 = json.Unmarshal(bad, &u2)
    fmt.Printf("v1: %q err=%v\nv2: %q err=%v\n", u1.CustomerID, err1, u2.CustomerID, err2)
    // 5) omitempty cannot leave out a zero time.Time; omitzero can.
    s1, _ := jsonv1.Marshal(shipment{})
    fmt.Printf("%s\n", s1)
}
```
`go run ./labs/10-jsonv2````
v1:      {"customerId":"ada","coupons":null}
v2:      {"customerId":"ada","coupons":[]}
v2+null: {"customerId":"ada","coupons":null}
v1: "mallory" err=<nil>
v2: "ada" err=<nil>
v1: "mallory" err=<nil>
v2: "ada" err=jsontext: duplicate object member name "customerId"
v2+allow: "mallory" err=<nil>
v1: "ad�a" err=<nil>
v2: "" err=jsontext: invalid UTF-8 within "/customerId" after offset 17
{"shippedAt":"0001-01-01T00:00:00Z"}
```
v2 writes a nil slice as `[]`; if you need the v1 behaviour, there is the `json.FormatNilSliceAsNull(true)` option. It matches field names case-sensitively, so the `CUSTOMERID` from Guess First no longer overwrites `customerId`. It rejects an object with the same name twice and invalid UTF-8; v1 took the last value in one case and silently turned the invalid byte into a `�` character in the other. Options are not a single object like C#'s `JsonSerializerOptions` but values you can pass to each call: `json.Marshal(v, opts...)`. Bookstore's API has to compile with Go 1.26 too, so it stays on v1; we will discuss moving to v2 in post 14.

## Money: Not float64

Since post 03 we have kept prices in kuruş in the `int64`-based `Money` type. Go has no built-in decimal type like .NET's `decimal`, and `float64` works in base two, so it cannot hold values like 349.90 exactly:

```
// Lab 10: three ways to carry a price in JSON.
package main
import (
    "encoding/json"
    "fmt"
    "bookstore/internal/catalog"
)
// Decimal shows a price as a string such as "349.90" in JSON.
type Decimal catalog.Money
// MarshalText makes encoding/json write the value as a JSON string.
func (d Decimal) MarshalText() ([]byte, error) {
    return fmt.Appendf(nil, "%d.%02d", d/100, d%100), nil
}
func main() {
    // float64: 3 x Dune plus 1 x Emma, then the 10% institutional discount.
    dune, emma, rate := 349.90, 199.00, 0.9 // variables: no exact constant arithmetic
    subtotal := dune*3 + emma
    f := subtotal * rate
    fmt.Println("float64:", subtotal, f)
    // int kuruş: the same calculation; the rounding rule is written down.
    sub := catalog.Money(34990)*3 + 19900
    k := sub * 90 / 100 // integer division: rounds down
    fmt.Println("kurus:  ", int64(sub), int64(k), k)
    out, _ := json.Marshal(map[string]any{"float": f, "kurus": k, "decimal": Decimal(k)})
    fmt.Println(string(out))
}
```
`go run ./labs/10-money````
float64: 1248.6999999999998 1123.83
kurus:   124870 112383 1123.83 TRY
{"decimal":"1123.83","float":1123.83,"kurus":112383}
```
The subtotal came out as `1248.6999999999998`. The final result looked right in this example, but in a report that adds up thousands of lines these small differences turn into kuruş. The API has two options. Sending kuruş as an integer (`34990`) is simple and lossless; the client divides by 100 for display. Sending the value as a string such as `"349.90"` is readable and lets the client parse it into its own decimal type without loss. The .NET API writes `decimal` as a JSON number (`349.90`); since JavaScript reads that as a `float64` again, the same problem can show up elsewhere on the Angular side. Bookstore's Go API stays with kuruş; this decision is part of the API contract and should be documented.

## In Production

Sending the decoder's error message to the client as it is leaks internal details: `placeOrderRequest.items.0.quantity` contains a Go type name, and in 1.27 the text of v1's messages may change. In production it is better to catch `*json.UnmarshalTypeError` and `*json.SyntaxError` with `errors.As` and produce your own message; the `Field` of `UnmarshalTypeError` gives the JSON path.

The body limit does not have to be the same for every endpoint. 64 KB is more than enough for an order, while an endpoint that uploads bulk institutional orders may need a larger limit. That is why setting the limit at the handler level is more flexible than a middleware.

Moving to v2 means behaviour changes. A nil slice becoming `[]` will not break a client, but case sensitivity can: an old client sending `CustomerId` will look under v2 as if it sent an empty value. Before switching, test with sample requests from real traffic.

## Common Pitfalls

Reading the body without a limit. `io.ReadAll(r.Body)` or an unlimited `json.NewDecoder(r.Body)` takes everything the client sends into memory. Always use `MaxBytesReader`.

Mixing up a missing value and zero. "No quantity given" and "quantity 0" are different errors; keep the difference with a pointer field or `omitzero`.

Carrying prices as `float64`. As we saw in the lab, intermediate results drift. Use kuruş or a string decimal.

Setting headers after the status. In `placeOrder`, `Location` and `Content-Type` are set before `WriteHeader(http.StatusCreated)`; set afterwards, they would not be sent.

Returning validation errors one at a time. Stopping at the first error forces the client to send the same form three times. List them all in one response with `errors.Join` and `ValidationErrors`.

## A Small Experiment

★☆☆ Send an order to `POST /api/orders`, then call the address in `Location` with `curl`. Which response do you get when you send the same body without a `Content-Type`?

★★☆ Add a branch for `*json.UnmarshalTypeError` to `writeDecodeError` and make it return `items[0].quantity: must be a number` inside `errors` when `"quantity":"2"` is sent.

★★★ Write a version that carries prices in JSON as `"349.90"` by adding `MarshalText`/`UnmarshalText` to `Money`. How many endpoints' responses changed? Is this change a breaking change for the client?

The API takes orders now, but the orders are lost when the process restarts. In the next post we connect to the same `BookstoreDb` with `database/sql` and `go-mssqldb`: connection pool settings, queries with a context, doing the order and the stock decrement in a single transaction, running the `sqlstore` from post 04 against a real SQL Server, and sqlc, the closest Go experience to Dapper.
