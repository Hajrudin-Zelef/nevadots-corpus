---
id: collect-261121-veille-ia/dotnet-csharp/c-15-unions-ndepend-blog
title: "C# 15 Unions - NDepend Blog"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["union", "type", "types", "unions", "compiler", "bird", "case", "switch"]
source: https://blog.ndepend.com/csharp-unions/
source_anchor: ""
source_lines: [1, 201]
section: "DotNet_CSharp"
---
A **C# union** is a type whose value is exactly one of a fixed, closed set of case types. You may also know it as a discriminated union, tagged union or sum type. Switch over it and the compiler checks that you handle every case, with no `default` arm needed. C# 15 ships this as the `union` keyword.

C# developers have asked for **C# unions** for years. **With C# 15 and .NET 11, released in November 2026, you can now use unions in your code**. This post explains how unions work, what the compiler generates for them and when to use them. It also covers a sibling feature that ships in C# 15: **closed class hierarchies**.

- C# unions ship with **C# 15 / .NET 11** (November 2026). C# 15 is the default language version for projects that target`net11.0` , so there is nothing to opt into.
- The `union` keyword composes existing types into a closed set. No shared base class or interface required.
- Pattern matching is exhaustive. Cover every case type and the switch compiles with no `default` arm.
- Under the hood a union is a plain `struct` holding one`object? Value` . Thus the runtime boxes value types, unless you hand-write a custom union with the non-boxing access pattern.
- `UnionAttribute` and`IUnion` live in`System.Runtime.CompilerServices` in the .NET 11 BCL. Any type that follows the*union pattern* gets the same compiler support as a`union` declaration.
- Unlike the `OneOf` NuGet package, unions plug into native C# pattern matching. As a result, the compiler checks exhaustiveness on every`switch` and`is` .

To use unions, simply target .NET 11 in your csproj:

| 1 | <TargetFramework>net11.0</TargetFramework> | 

Here is a first C# union in action. It declares a `Pet` union over three record types and switches over it:

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 | Pet pet = new Dog("Rover"); var description = pet switch {     Dog(var name) => name,     Cat(var adjective) => $"A {adjective} cat",     Bird bird => $"A {bird.Species}", }; public union Pet (Dog, Cat, Bird); public record class Dog(string Name); public record struct Cat(string Adjective); public record class Bird(string Species); public record class Shark(int Teeth); | 

This code feels very idiomatic to C#. Here are some remarks:

- `Pet` wraps the existing types`Dog` ,`Cat` , and`Bird` . Once declared, you no longer repeat the constituent types.`Pet` already carries that information.
- The first line, `Pet pet = new Dog("Rover");` , compiles thanks to an implicit*union conversion* . The language provides one from each case type to the union. On the other hand, the compiler rejects any value of an unlisted type. So`Pet pet = new Shark(20);` fails to compile.

Unlike some languages such as F#, C# does not let you declare the case types inside the union. Instead, you declare them on their own. For comparison, here is a union in F#. `Car`, `Truck`, and `Bicycle` belong to the `Vehicle` union. They act as tags for the various cases rather than as types of their own. This is why functional languages use the names **discriminated union** or **tagged union**. The C# team picked a different model, closer to a **structural union** or **type union**.

| 1 2 3 4 | type Vehicle =     \| Car of model : string * year : int     \| Truck of model : string * year : int * payload : float     \| Bicycle of model : string | 

A key benefit shows up in pattern matching. The compiler knows `Pet` only ever holds a `Dog`, `Cat`, or `Bird`. So a `switch` expression over `Pet` compiles without warning once all three arms are present. No `_` discard, no `default` arm. That is the whole point of a closed set of types.

Now extend the union later with `Shark`. The compiler then emits warning `CS8509` on every `switch` over `Pet` that misses the new case. This makes `Pet` a **closed union type**. The compiler fixes its set of possibilities and enforces them.

Class inheritance works the opposite way. You can add new subclasses without breaking existing code, thanks to polymorphism on virtual members. This is the Open-Close Principle (OCP): open for extension, closed for modification. In a word, OCP applies to class inheritance but not to unions.

The compiler turns a union declaration into a plain `struct`, not a `record struct`. This struct has a single read-only `object? Value` property and one public constructor per case type. It also carries the `[Union]` attribute and implements the `IUnion` interface. The design feels simple and powerful.

In C# virtually any type fits into an `object` reference. This includes classes, structs, interfaces, delegates, enumerations and primitive types. Type parameters, nullable types and even other unions work too. However, types that don’t convert to `object` cannot be case types. Examples are `ref struct` types and unmanaged pointers.

| 1 2 3 4 5 6 7 8 9 10 11 | #region public union Pet (Dog, Cat, Bird); [System.Runtime.CompilerServices.Union] public struct Pet : System.Runtime.CompilerServices.IUnion {     public Pet(Dog value) => Value = value;     public Pet(Cat value) => Value = value;     public Pet(Bird value) => Value = value;     public object? Value { get; } } #endregion | 

A few consequences are worth knowing:

- A union conversion simply calls the matching constructor. The compiler turns `Pet pet = dog;` into`Pet pet = new Pet(dog);` .
- A value might convert to more than one case type equally well. In that case the union conversion is ambiguous and the compiler reports an error. Case types may overlap, but the call site must not be ambiguous.
- A user-defined `implicit` conversion operator wins over a union conversion.
- A union conversion to a nullable union also works: `Pet? maybePet = new Dog("Buddy");` compiles.

Unions are not records. Hence the compiler doesn’t generate any equality operator, `ToString()` override or `Deconstruct()` method. For example, `pet1 == pet2` does not compile because the compiler turns a union into a plain struct, and C# never gives a struct an `==` operator by default. Classes get reference equality for free and records get a generated `==`, but a `struct` must declare operator `==` itself. Since Pet declares none, the compiler reports error CS0019.

Also unions’ `Equals()` falls back to the default `ValueType` implementation. Also, `pet.ToString()` returns the type name, not the content. Add those members to the union body yourself if you need them.

You never need to switch on `pet.Value`. Instead, you switch on the union itself, and the compiler *unwraps* it for the patterns where this makes sense:

- **Type and declaration patterns** such as`Dog` or`Dog d` test the content, not the union instance.
- **Typed property or positional patterns** such as`Dog { Name: "Rex" }` first test the content against the type. Then they apply the nested patterns to that case value.
- **Constant and relational patterns** such as`null` ,`42` or`> 0` test the content.
- **`var` and discard `_` patterns** capture or match the union instance itself, without unwrapping.
- **Untyped property patterns** such as`{ Name: "Rex" }` and**list patterns** use the members of the union instance itself.

These rules are the same for `is` and `switch`. Note that a type pattern must be compatible with at least one case type. So both `pet is Pet` and `pet is Shark` produce error `CS8121`.

| 1 2 3 4 5 6 | Pet pet = new Dog("Rex"); Console.WriteLine(pet is Dog { Name: "Rex" });  // True, tested on the Dog content Console.WriteLine(pet is Cat);                  // False Console.WriteLine(pet is not null);             // True, Value is not null Console.WriteLine(pet is var p && p.Value is Dog); // True, 'var' captures the Pet itself | 

A union is a struct, so it has a `default` value where the underlying `Value` is null. This also holds for a default-initialized field or array element. Here, a `null` pattern checks whether `Value` is null:

| 1 2 3 4 5 6 7 8 9 | Pet pet = default; var description = pet switch {     Dog d   => d.Name,     Cat c   => c.Adjective,     Bird b  => b.Species,     null    => "no pet", }; // description is "no pet" | 

Nullable analysis drives the rules:

- When every case type is non-nullable, the compiler considers `Value` as not-null. A switch covering all case types is exhaustive, and you get no warning without a`null` arm. But be aware: if that switch receives a`default` union at runtime, it throws a`SwitchExpressionException` .
- Now suppose one case type is itself nullable, like `Bird?` ,`int?` or an unconstrained`T` . Then`Value` becomes maybe-null, and omitting the`null` arm produces warning`CS8655` .
- Prefer a `null` arm over a`_` or`var` arm to handle the empty case. A discard also swallows any case type you add later. Consequently, the compiler can no longer warn you about it.

The `UnionAttribute` and the `IUnion` interface both live in `System.Runtime.CompilerServices`. Since .NET 11, the runtime ships them, and they look like this:

| 1 2 3 4 5 6 7 8 9 10 11 12 | namespace System.Runtime.CompilerServices {     [AttributeUsage(AttributeTargets.Class \| AttributeTargets.Struct,                     AllowMultiple = false)]     public sealed class UnionAttribute : Attribute;     public interface IUnion     {         // The value of the union or null         object? Value { get; }     } } | 

Here is the important point: this is a public contract. The compiler treats any class or struct as a union when it follows three rules:

- It carries the `[Union]` attribute.
- It exposes one or more public constructors with a single parameter. Each parameter type defines a case type.
- It exposes a public `Value` property of type`object?` or`object` .

Compiler-generated unions also implement `IUnion`. Do the same in hand-written ones, so that generic code can inspect any union at runtime. For example, `if (obj is IUnion { Value: null })` works on all unions. In other words, the `union` keyword is sugar over a pattern that anyone can implement by hand. We will come back to that.

Unions are a compiler feature. So you can still use them when you multi-target older frameworks like `net8.0` or `net48`. For those targets, set the language version explicitly and declare the two types yourself. Keep in mind that Microsoft doesn’t officially support a newer C# version on an older framework.

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 | // In the csproj: <LangVersion>latest</LangVersion> #if !NET11_0_OR_GREATER namespace System.Runtime.CompilerServices {     [AttributeUsage(AttributeTargets.Class \| AttributeTargets.Struct,                     AllowMultiple = false)]     public sealed class UnionAttribute : Attribute;     public interface IUnion     {         object? Value { get; }     } } #endif | 

A union declaration goes beyond its case list. It can also carry a body. That body is handy when you want helper methods that hide the switch from callers:

| 1 2 3 4 5 6 7 8 9 | public union OneOrMore<T>(T, IEnumerable<T>) {     public IEnumerable<T> AsEnumerable() => Value switch     {         T single             => [single],         IEnumerable<T> many  => many,         null                 => []     }; } | 

A few things are worth pointing out in that snippet:

- Unions can be **generic** .`OneOrMore<T>` is a single declaration that works for any`T` . Like a struct, a union can also declare type parameter constraints and implement interfaces.
- Inside the body, `Value` is maybe-null here, since`T` has no constraint and could be a nullable type. Hence the`null` arm. That matches reality: a default`OneOrMore<T>` has a null`Value` . Within the body you can also write`this switch { ... }` , with the same unwrapping rules.

The body accepts the same members as a struct, with a few restrictions:

- No instance fields, auto-properties or field-like events, since all the state lives in `Value` .
- No public constructor with a single parameter, because the compiler generates those.
- Any other constructor must delegate to a generated one through a `this(...)` initializer, directly or indirectly.

The combination “single item or collection of items” is a real pain point in API design. Until now the choices were ugly. You could take `IEnumerable<T>` everywhere and force callers to wrap. Alternatively, you could take `object` and write defensive code, or define overloads. With `OneOrMore<T>` the API tells the truth about what it accepts. Meanwhile the helper hides the dispatch:

| 1 2 3 4 5 | OneOrMore<string> singleTag = "dotnet"; OneOrMore<string> manyTags = new[] { "csharp", "unions", "dotnet11" }; foreach (var tag in singleTag.AsEnumerable())     Console.Write($"[{tag}] ");   // [dotnet] | 

I suspect most real-world usage will look like this kind of small union with a helper. Not the textbook `Pet` example.

Storing everything in an `object?` has a trade-off. The runtime boxes any value type listed in the union, such as `Cat`, the moment you write it into `Value`. The garbage collector then has to manage that box, which adds a small performance overhead.

A smarter implementation could optimize storage for value types that share the same memory footprint. Alternatively, the runtime could allocate space based on the largest value type in the union. Smaller types would then carry padding overhead. Either approach would significantly complicate things.

Instead, C# 15 lets you define the layout of a value-type-only union by hand. That is exactly the door `[Union]` + `IUnion` opens. You write a custom `struct` and tag it with `[Union]`. Inside, you store the cases as discriminated fields, not as a boxed `object?`. The compiler still checks exhaustiveness on switches over your type. Yet you keep control of the allocation story.

To make this work, the compiler recognises an optional **non-boxing access pattern** on a custom union:

- A `bool HasValue { get; }` property that returns`true` when`Value` is not null. The compiler uses it for the`null` pattern.
- One `bool TryGetValue(out T value)` overload per case type. It returns`true` only when the content is a non-null value of that case type. The compiler uses it for type patterns.

When these members are present, the compiler calls them instead of going through `Value`. Each member applies to its own pattern kind. A switch over the union then never boxes.

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 | using System.Diagnostics.CodeAnalysis; using System.Runtime.CompilerServices; [Union] public readonly struct IntOrString : IUnion {    private readonly byte _kind; // 0 = empty, 1 = int, 2 = string    private readonly int _int;    private readonly string? _string;    public IntOrString(int value) { _kind = 1; _int = value; }    public IntOrString(string? value) {       if (value is not null) { _kind = 2; _string = value; }    }    // Required by the union pattern and IUnion. Boxes an int, but only used as a fallback.    public object? Value => _kind switch { 1 => _int, 2 => _string, _ => null };    // Non-boxing access pattern    public bool HasValue => _kind != 0;    public bool TryGetValue(out int value) {       value = _int;       return _kind == 1;    }    public bool TryGetValue([MaybeNullWhen(false)] out string value) {       value = _string;       return _kind == 2;    } } | 

Usage looks exactly like a `union` declaration. Union conversions and exhaustiveness work the same:

| 1 2 3 4 5 6 7 | IntOrString v = 42;              // calls new IntOrString(42) string text = v switch {    int i    => $"int: {i}",       // calls TryGetValue(out int), no boxing    string s => $"string: {s}",    // calls TryGetValue(out string)    null     => "empty"            // calls HasValue }; | 

Notice that the `string?` constructor leaves the union empty when it receives `null`. Indeed, the compiler assumes a few well-formedness rules from a custom union, without verifying them:

- `Value` always returns null or a value of a case type, even for`default` .
- A union you create from a case type holds that case type, or null if you passed null.
- `HasValue` and`TryGetValue` behave exactly like checking`Value` .

This escape hatch matters. Unions made solely of value types form a common scenario. Think parser implementation, messaging protocol, financial instruments. Avoiding allocations there matters. In the same vein C# 13 eliminated object allocations for `params` collections.

Modern C# and `MemoryMarshal` make it easy to cast a range of bytes into any value type instance. This comes in handy when you build such a hand-written value-only union:

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 | using System.Diagnostics; using System.Runtime.InteropServices; Span<byte> bytes8 = stackalloc byte[8] { 2, 0, 0, 0, 1, 0, 0, 0 }; // Convert first 4 bytes to uint uint value32 = MemoryMarshal.Read<uint>(bytes8); Debug.Assert(value32 == 2); // Convert all 8 bytes to ulong ulong value64 = MemoryMarshal.Read<ulong>(bytes8); Debug.Assert(value64 == 4294967298); // Convert all 8 bytes to MyStruct ref MyStruct myStruct = ref MemoryMarshal.AsRef<MyStruct>(bytes8); Debug.Assert(myStruct.X == 2); Debug.Assert(myStruct.Y == 1); [StructLayout(LayoutKind.Sequential)] struct MyStruct { public uint X; public uint Y; } | 

Sometimes public single-parameter constructors are not what you want. You might need a private constructor, some validation or a factory. Or your type is a `record class`, and the compiler would pick up its copy constructor as a union constructor. For these cases C# 15 supports **union member providers**.

The union type declares and implements a public nested interface named `IUnionMembers`. When this interface is present, the compiler looks up union members only there:

- One static `Create` factory method per case type, taking a single parameter and returning the union type.
- A `Value` property of type`object?` or`object` .
- Optionally, `HasValue` and`TryGetValue` overloads for the non-boxing access pattern.

| 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 | [Union] public sealed class Result<T> : Result<T>.IUnionMembers {    private readonly object? _value;    private Result(object? value) => _value = value;    public interface IUnionMembers {       static Result<T> Create(T value) => new(value);       static Result<T> Create(Exception error) => new(error);       object? Value { get; }    }    object? IUnionMembers.Value => _value; } // Compiled as Result<int>.IUnionMembers.Create(42) Result<int> result = 42; string text = result switch {    int n       => $"OK: {n}",    Exception e => $"Error: {e.Message}",    null        => "nothing" }; | 

This sample also shows that a custom union can be a class. In that case, the `null` pattern succeeds when either the reference itself or its `Value` is null.

C# 15 ships another way to get exhaustiveness: the `closed` modifier on a class. A `closed` class is implicitly `abstract`. Moreover, only the declaring assembly can define its direct derived types. The compiler therefore knows every direct subtype and checks switches for exhaustiveness:

| 1 2 3 4 5 6 7 8 9 10 11 12 | public closed record class JobStatus; public record class Queued : JobStatus; public record class Running(int PercentComplete) : JobStatus; public record class Completed(TimeSpan Elapsed) : JobStatus; public record class Failed(string Error) : JobStatus; string Describe(JobStatus status) => status switch {    Queued              => "queued",    Running(var pct)    => $"{pct}% done",    Completed(var time) => $"done in {time}",    Failed(var error)   => $"failed: {error}", };   // no discard needed | 

How to choose? Use a **closed hierarchy** when you own the case types and they share data or behavior through a base class. It also gives you reference semantics. On the contrary, use a **union** to compose types that have nothing in common or that you don’t own, like `int`, `string` or `Exception`.

The team discussed a few items that are not part of the C# 15 release:

- **Closed enums.** They remain on the roadmap for a future version. A closed enum prevents creating values other than the declared members. Thus it allows exhaustive switches without a discard.
- **Record unions.** A union declaration lowers to a plain struct. There is no`record union` and no generated value equality,`ToString()` or deconstruction.
- **BCL union types.** The .NET 11 BCL ships the plumbing (`UnionAttribute` ,`IUnion` ) but no general-purpose`Option<T>` or`Result<T, TError>` types. For now you declare your own, which takes one line.
- **F# interop.** C# doesn’t see F# discriminated unions as C# unions, and F# doesn’t see C# unions as F# unions either.

Visual Studio 2026 supports unions, and so does Visual Studio Code with the C# Dev Kit.

Before unions, C# developers reached for the `OneOf` NuGet package, an abstract base class, or an `enum` plus a payload. Here is how the native `union` compares:

| Approach | Closed set | Compiler exhaustiveness | No inheritance needed | 
|---|---|---|---|
| C# 15 `union` | Yes | Yes | Yes | 
| C# 15 `closed` class | Yes | Yes | No | 
| `OneOf<...>` NuGet | Yes | Only through `Match()` callbacks | Yes | 
| Class / interface hierarchy | No (open) | No | No | 
| `enum` | No (any integer can be cast) | Partial | n/a | 

The win is the middle column. With `OneOf` you get exhaustiveness only through its `Match()` / `Switch()` methods and their lambdas. In contrast, the `union` keyword and `closed` classes turn “did I handle every case?” into a compile-time check. That check applies to regular C# `switch` and `is` pattern matching.

A union is a type whose value is exactly one of a fixed set of case types. These can be classes, structs, records, interfaces or primitives. The compiler enforces that set and checks that every switch handles all cases. People also call the concept a discriminated union, tagged union or sum type.

Unions shipped with C# 15 and .NET 11 in November 2026. C# 15 is the default language version for projects that target `net11.0`.

F# declares the case tags inside the union. C# composes existing standalone types instead. Hence its model is closer to a structural or type union than to a tagged union. Both give exhaustive pattern matching.

Yes, for a `union` declaration. The generated struct stores its content in a single `object?` property, so the runtime boxes a struct case. You avoid that by hand-writing a `[Union]` struct. Store the cases in typed fields and expose `HasValue` and `TryGetValue` members.

Target `net11.0` and declare `public union MyUnion(TypeA, TypeB);`. `UnionAttribute` and `IUnion` are part of the .NET 11 runtime. For older target frameworks, set the language version and declare these two types yourself.

Use a `closed` class when you own the case types and they share a base class. Use a `union` to combine unrelated existing types such as `int`, `string` or `Exception` without any inheritance.

They cover the same need but at the language level. `OneOf` enforces exhaustiveness only through its `Match()` callbacks. On the other hand, the `union` keyword gives compiler-enforced exhaustiveness on native `switch` and `is` pattern matching.

Unions are the headline feature of C# 15. The syntax is concise and feels natural, and pattern matching works on the union directly. Generic unions and union bodies turn the feature into something useful for everyday API design.

C# 15 also takes care of performance-sensitive code. The `[Union]` / `IUnion` pattern lets you write value-type-only unions that never box, thanks to non-boxing access members and union member providers. Finally, `closed` class hierarchies give the same exhaustiveness guarantee to classic object-oriented designs.

There is still room for more. Closed enums, BCL-provided `Option` and `Result` types and an F# interop story are natural next steps.

In 2024 we already wrote a blog post to explain the concept to C# developers: C# Discriminated Union: What’s Driving the C# Community’s Inquiries?. Now you can finally put the concept into practice.
