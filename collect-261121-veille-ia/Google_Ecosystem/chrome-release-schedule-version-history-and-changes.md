---
id: collect-261121-veille-ia/google-ecosystem/chrome-release-schedule-version-history-and-changes
title: "Chrome release schedule, version history and changes"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-17"]
keywords: ["chrome", "version", "changes", "stable", "changed", "fingerprinting", "automationstable", "high"]
source: https://www.clearcotelabs.com/chrome-releases
source_anchor: ""
source_lines: [1, 366]
section: "Google_Ecosystem"
---
# What each Chrome release changed

Chrome ships a new version every two weeks, and each one changes something a website or a server can see: new APIs, removed ones, Client Hints, the network handshake. Each page here lists those changes for one version, measured on real builds, with a one-line check and a Chromium source link for every item.

- Chrome 157 → 158## Chrome 158: what changed for fingerprinting and automationStable Nov 17, 2026 · 21 changes (4 high, 4 medium, 13 low impact)
- Chrome 156 → 157## Chrome 157: what changed for fingerprinting and automationStable Nov 3, 2026 · 18 changes (5 high, 4 medium, 9 low impact)
- Chrome 155 → 156## Chrome 156: what changed for fingerprinting and automationStable Oct 20, 2026 · 23 changes (6 high, 14 medium, 3 low impact)
- Chrome 154 → 155## Chrome 155: what changed for fingerprinting and automationStable Oct 6, 2026 · 29 changes (6 high, 13 medium, 10 low impact)
- Chrome 153 → 154## Chrome 154: what changed for fingerprinting and automationStable Sep 22, 2026 · 16 changes (7 high, 5 medium, 4 low impact)
- Chrome 152 → 153## Chrome 153: what changed for fingerprinting and automationStable Sep 8, 2026 · 18 changes (6 high, 8 medium, 4 low impact)
- Chrome 151 → 152## Chrome 152: what changed for fingerprinting and automationStable Aug 25, 2026 · 43 changes (16 high, 22 medium, 5 low impact)

## Chrome release schedule

Chrome 158 is the current stable version, since Nov 17, 2026. Chrome 159 reaches stable on Dec 8, 2026, after an early rollout to a small share of users from Nov 18, 2026. Since Chrome 153, a new version reaches stable every two weeks (it was every four weeks before), with longer gaps around the end of the year. Each version is cut from Chromium's main branch at its branch point, goes to beta, then to early stable, then to everyone over the days after its stable date.

| Chrome | Branch point | First beta | Early stable | Stable | Gap | Status | 
|---|---|---|---|---|---|---|
| Chrome 158 | Oct 26, 2026 | Oct 28, 2026 | Nov 4, 2026 | Nov 17, 2026 | 2 wk | Stable now | 
| Chrome 159 | Nov 9, 2026 | Nov 11, 2026 | Nov 18, 2026 | Dec 8, 2026 | 3 wk | Early stable | 
| Chrome 160LTS | Nov 30, 2026 | Dec 2, 2026 | Dec 9, 2026 | Jan 5, 2027 | 4 wk | Upcoming | 
| Chrome 161 | Dec 14, 2026 | Dec 16, 2026 | Jan 6, 2027 | Jan 19, 2027 | 2 wk | Upcoming | 
| Chrome 162 | Jan 11, 2027 | Jan 13, 2027 | Jan 20, 2027 | Feb 2, 2027 | 2 wk | Upcoming | 
| Chrome 163 | Jan 25, 2027 | Jan 27, 2027 | Feb 3, 2027 | Feb 16, 2027 | 2 wk | Upcoming | 
| Chrome 164 | Feb 8, 2027 | Feb 10, 2027 | Feb 17, 2027 | Mar 2, 2027 | 2 wk | Upcoming | 

From Google's Chromium Dash, checked every six hours. Dates further ahead are Google's plan and can move.

## Every check across the series, and which version your browser matches

One row per check, one column per Chrome version. A tinted cell differs from the version before it, so a row tinted in one column only identifies that version. Run the checks to see where this browser's engine lands.

Chrome 151 → Chrome 152 → Chrome 153 → Chrome 154 → Chrome 155 → Chrome 156 → Chrome 157 → Chrome 158 · 236 checks

Run the checks to see which Chrome version this browser's engine matches.

Where the changes are. Select a surface to filter the rows.

| Check | Chrome 151 | Chrome 152 | Chrome 153 | Chrome 154 | Chrome 155 | Chrome 156 | Chrome 157 | Chrome 158 | This browser | 
|---|---|---|---|---|---|---|---|---|---|
| TLS handshake |  |  |  |  |  |  |  |  |  | 
| TLS ClientHello gains the trust_anchors extension and a GREASE signature algorithm `JSON.parse(document.querySelector('pre').textContent).tls.ja4` | "t13d1516h2_8daaf6152771_806a8c22fdea" | "t13d1517h2_8daaf6152771_cb7bf5808d99" |  |  |  |  |  |  | n/a | 
| TLS ClientHello gains the trust_anchors extension and a GREASE signature algorithm `JSON.parse(document.querySelector('pre').textContent).tls.extensions.some((e) => e.name === 'Unknown extension 51764')` | false | true |  |  |  |  |  |  | n/a | 
| Header order |  |  |  |  |  |  |  |  |  | 
| Connection-Allowlist response header is enforced for every site `fetch('https://example.com/', { mode: 'no-cors' }).then(() => 'sent', (e) => e.name)` | "sent" | "TypeError" |  |  |  |  |  |  | n/a | 
| Topics and Attribution Reporting request options and headers removed `(() => { let read = false; new Request('/', { get attributionReporting() { read = true; } }); return read; })()` | true | false |  |  |  |  |  |  | – | 
| Topics and Attribution Reporting request options and headers removed `(() => { let read = false; new Request('/', { get browsingTopics() { read = true; } }); return read; })()` | true | false |  |  |  |  |  |  | – | 
| JPEG XL decoded by default, and image/jxl added to the Accept header `ImageDecoder.isTypeSupported('image/jxl')` |  |  |  | false | true |  |  |  | – | 
| Client Hints |  |  |  |  |  |  |  |  |  | 
| New GREASE brand and brand order in User-Agent Client Hints ``navigator.userAgentData.brands.map((b) => `${b.brand};v=${b.version}`).join(', ')`` | "Not=A?Brand;v=99, Google Chrome;v=151, Chromium;v=151" | "Chromium;v=152, Not?A_Brand;v=24, Google Chrome;v=152" | "Google Chrome;v=153, Not_A Brand;v=8, Chromium;v=153" | "Chromium;v=154, Google Chrome;v=154, Not A(Brand;v=99" | "Google Chrome;v=155, Chromium;v=155, Not(A:Brand;v=24" | "Not:A-Brand;v=8, Chromium;v=156, Google Chrome;v=156" | "Not-A.Brand;v=99, Google Chrome;v=157, Chromium;v=157" | "Chromium;v=158, Not.A/Brand;v=24, Google Chrome;v=158" | – | 
| JavaScript and DOM |  |  |  |  |  |  |  |  |  | 
| navigator.cpuPerformance (CPU Performance API) `'cpuPerformance' in navigator` | false | true |  |  |  |  |  |  | – | 
| navigator.cpuPerformance (CPU Performance API) `typeof navigator.cpuPerformance` | "undefined" | "number" |  |  |  |  |  |  | – | 
| OpaqueRange, NodeRange and createValueRange(): the Range prototype chain changes `typeof OpaqueRange` | "undefined" | "function" |  |  |  |  |  |  | – | 
| OpaqueRange, NodeRange and createValueRange(): the Range prototype chain changes `Object.getPrototypeOf(Range.prototype).constructor.name` | "AbstractRange" | "NodeRange" |  |  |  |  |  |  | – | 
| OpaqueRange, NodeRange and createValueRange(): the Range prototype chain changes `'startContainer' in AbstractRange.prototype` | true | false |  |  |  |  |  |  | – | 
| OpaqueRange, NodeRange and createValueRange(): the Range prototype chain changes `typeof HTMLInputElement.prototype.createValueRange` | "undefined" | "function" |  |  |  |  |  |  | – | 
| FeaturePolicy is renamed PermissionsPolicy `typeof PermissionsPolicy` | "undefined" | "function" |  |  |  |  |  |  | – | 
| FeaturePolicy is renamed PermissionsPolicy `FeaturePolicy.name` | "FeaturePolicy" | "PermissionsPolicy" |  |  |  |  |  |  | – | 
| FeaturePolicy is renamed PermissionsPolicy `Object.prototype.toString.call(document.featurePolicy)` | "[object FeaturePolicy]" | "[object PermissionsPolicy]" |  |  |  |  |  |  | – | 
| window.chrome is now a configurable property, created later `Object.getOwnPropertyDescriptor(window, 'chrome').configurable` | false | true |  |  |  |  |  |  | – | 
| window.chrome is now a configurable property, created later `Object.getOwnPropertyNames(window).indexOf('chrome') < Object.getOwnPropertyNames(window).indexOf('WebAssembly')` | true | false |  |  |  |  |  |  | – | 
| Protected Audience, Topics and Shared Storage are stubbed out: still exposed, no longer working `(() => { try { return navigator.adAuctionComponents(1).length; } catch (e) { return e.name; } })()` | "InvalidStateError" | 0 |  |  |  |  |  |  | – | 
| Protected Audience, Topics and Shared Storage are stubbed out: still exposed, no longer working `document.browsingTopics().then(() => 'resolved', (e) => e.name)` | "resolved" | "NotSupportedError" |  |  |  |  |  |  | – | 
| Protected Audience, Topics and Shared Storage are stubbed out: still exposed, no longer working `navigator.protectedAudience.queryFeatureSupport('sellerNonce')` | true | false |  |  |  |  |  |  | – | 
| Protected Audience, Topics and Shared Storage are stubbed out: still exposed, no longer working `navigator.canLoadAdAuctionFencedFrame()` | true | false |  |  |  |  |  |  | n/a | 
| DeviceOrientationEvent.requestPermission() and DeviceMotionEvent.requestPermission() `typeof DeviceOrientationEvent.requestPermission` | "undefined" | "function" |  |  |  |  |  |  | – | 
| DeviceOrientationEvent.requestPermission() and DeviceMotionEvent.requestPermission() `typeof DeviceMotionEvent.requestPermission` | "undefined" | "function" |  |  |  |  |  |  | – | 
| console.context() objects get dirxml instead of dirXml `typeof console.context().dirxml` | "undefined" | "function" |  |  |  |  |  |  | – | 
| console.context() objects get dirxml instead of dirXml `typeof console.context().dirXml` | "function" | "undefined" |  |  |  |  |  |  | – | 
| V8's internal built-in closures are strict: .caller throws, f.caller is null `(() => { try { return typeof Proxy.revocable({}, {}).revoke.caller; } catch (e) { return e.name; } })()` | "object" | "TypeError" |  |  |  |  |  |  | – | 
| V8's internal built-in closures are strict: .caller throws, f.caller is null `new Promise((r) => Promise.resolve().finally(function f() { r(typeof f.caller); }))` | "function" | "object" |  |  |  |  |  |  | n/a | 
| Property order moved on window, Document, PointerEvent and the Performance interfaces `Object.getOwnPropertyNames(window).indexOf('onpagereveal') < Object.getOwnPropertyNames(window).indexOf('onpageswap')` | false | true |  |  |  |  |  |  | – | 
| Property order moved on window, Document, PointerEvent and the Performance interfaces `Object.getOwnPropertyNames(PointerEvent.prototype).indexOf('persistentDeviceId') < Object.getOwnPropertyNames(PointerEvent.prototype).indexOf('getPredictedEvents')` | false | true |  |  |  |  |  |  | – | 
| Property order moved on window, Document, PointerEvent and the Performance interfaces `Object.getOwnPropertyNames(PerformanceNavigationTiming.prototype).indexOf('confidence') < Object.getOwnPropertyNames(PerformanceNavigationTiming.prototype).indexOf('toJSON')` | false | true |  |  |  |  |  |  | – | 
| Property order moved on window, Document, PointerEvent and the Performance interfaces `Object.getOwnPropertyNames(PerformancePaintTiming.prototype)[0]` | "paintTime" | "toJSON" |  |  |  | "toJSON" | "paintTime" |  | – | 
| Property order moved on window, Document, PointerEvent and the Performance interfaces `Object.getOwnPropertyNames(window).indexOf('PerformanceLongAnimationFrameTiming') < Object.getOwnPropertyNames(window).indexOf('Performance')` | true | false |  |  |  |  |  |  | – | 
| The attribution-reporting permissions-policy feature is removed `document.featurePolicy.features().includes('attribution-reporting')` | true | false |  |  |  |  |  |  | – | 
| Shadow DOM reference target: ShadowRoot.referenceTarget `'referenceTarget' in ShadowRoot.prototype` | false | true |  |  |  |  |  |  | – | 
| Shadow DOM reference target: ShadowRoot.referenceTarget `'shadowRootReferenceTarget' in HTMLTemplateElement.prototype` | false | true |  |  |  |  |  |  | – | 
| Declarative shadow DOM: shadowrootslotassignment `'shadowRootSlotAssignment' in HTMLTemplateElement.prototype` | false | true |  |  |  |  |  |  | – | 
| Sanitizer API: processing instructions in the configuration, streaming sanitizer on `'processingInstructions' in new Sanitizer().get()` | false | true |  |  |  |  |  |  | – | 
| Sanitizer API: processing instructions in the configuration, streaming sanitizer on `Document.parseHTML('a<!--x-->b').body.childNodes.length` | 2 | 1 |  |  |  |  |  |  | – | 
| RegExp split with the v flag steps by code point on the spec path `(() => { const v = []; class R extends RegExp { exec() { v.push(this.lastIndex); return null; } } ('a' + String.fromCodePoint(128512) + 'b').split(new R('x', 'v')); return v.join(); })()` | "0,1,2,3" | "0,1,3" |  |  |  |  |  |  | – | 
| WebAssembly compile errors name --wasm-* flags instead of --experimental-wasm-* `(() => { try { new WebAssembly.Module(new Uint8Array([0, 97, 115, 109, 1, 0, 0, 0, 1, 4, 1, 96, 0, 0, 3, 2, 1, 0, 10, 6, 1, 4, 0, 224, 0, 11])); return 'compiled'; } catch (e) { return e.message.slice(e.message.indexOf('(enable')); } })()` | "(enable with --experimental-wasm-wasmfx) @+23" | "(enable with --wasm-wasmfx) @+23" |  |  |  |  |  |  | – | 
| Error stacks include WebAssembly frames across JSPI suspensions `(async () => { const b = new Uint8Array([0, 97, 115, 109, 1, 0, 0, 0, 1, 4, 1, 96, 0, 0, 2, 7, 1, 1, 109, 1, 102, 0, 0, 3, 2, 1, 0, 7, 7, 1, 3, 114, 117, 110, 0, 1, 10, 6, 1, 4, 0, 16, 0, 11]); const { instance: i } = await WebAssembly.instantiate(b, { m: { f: new WebAssembly.Suspending(async () => { await 0; throw Error(); }) } }); try { await WebAssembly.promising(i.exports.run)(); } catch (e) { return e.stack.includes('wasm-function'); } })()` | false | true |  |  |  |  |  |  | – | 
| New Iterator helpers: Iterator.zip, Iterator.zipKeyed and Iterator.prototype.join `typeof Iterator.zip` |  | "undefined" | "function" |  |  |  |  |  | – | 
| New Iterator helpers: Iterator.zip, Iterator.zipKeyed and Iterator.prototype.join `typeof Iterator.zipKeyed` |  | "undefined" | "function" |  |  |  |  |  | – | 
| New Iterator helpers: Iterator.zip, Iterator.zipKeyed and Iterator.prototype.join `typeof Iterator.prototype.join` |  | "undefined" | "function" |  |  |  |  |  | – | 
| New <camera> and <microphone> elements `typeof HTMLCameraElement` |  | "undefined" | "function" |  |  |  |  |  | – | 
| New <camera> and <microphone> elements `typeof HTMLMicrophoneElement` |  | "undefined" | "function" |  |  |  |  |  | – | 
| Shared Storage removed, with its permissions-policy features `'sharedStorage' in window` |  | true | false |  |  |  |  |  | – | 
| Shared Storage removed, with its permissions-policy features `typeof SharedStorageWorklet` |  | "function" | "undefined" |  |  |  |  |  | – | 
| Shared Storage removed, with its permissions-policy features `'sharedStorageWritable' in HTMLIFrameElement.prototype` |  | true | false |  |  |  |  |  | – | 
| Shared Storage removed, with its permissions-policy features `'canLoadOpaqueURL' in HTMLFencedFrameElement` |  | true | false |  |  |  |  |  | – | 
| Shared Storage removed, with its permissions-policy features `document.featurePolicy.features().includes('run-ad-auction')` |  | true | false |  |  |  |  |  | – | 
| HTMLElement.autocorrect `'autocorrect' in HTMLElement.prototype` |  | false | true |  |  |  |  |  | – | 
| BaseAudioContext.renderQuantumSize `'renderQuantumSize' in BaseAudioContext.prototype` |  | false | true |  |  |  |  |  | – | 
| Property order moved on Element, Document and CustomElementRegistry `Object.getOwnPropertyNames(Element.prototype).indexOf('customElementRegistry') < Object.getOwnPropertyNames(Element.prototype).indexOf('onfullscreenchange')` |  | false | true |  |  |  |  |  | – | 
| Property order moved on Element, Document and CustomElementRegistry `Object.getOwnPropertyNames(Document.prototype).indexOf('customElementRegistry') < Object.getOwnPropertyNames(Document.prototype).indexOf('fonts')` |  | false | true |  |  |  |  |  | – | 
| Property order moved on Element, Document and CustomElementRegistry `Object.getOwnPropertyNames(CustomElementRegistry.prototype).indexOf('initialize') < Object.getOwnPropertyNames(CustomElementRegistry.prototype).indexOf('upgrade')` |  | false | true |  |  |  |  |  | – | 
| RTCDataChannel options now reject out-of-range values `(() => { const pc = new RTCPeerConnection(); try { pc.createDataChannel('x', { maxRetransmits: -1 }); return 'no throw'; } catch (e) { return e.name; } finally { pc.close(); } })()` |  | "no throw" | "TypeError" |  |  |  |  |  | – | 
| V8 15.4 reorders the global object: Float16Array, SuppressedError and friends move to their spec positions `Object.getOwnPropertyNames(window)[Object.getOwnPropertyNames(window).indexOf('Float64Array') + 1]` |  |  | "DataView" | "Float16Array" |  |  |  |  | – | 
| V8 15.4 reorders the global object: Float16Array, SuppressedError and friends move to their spec positions `Object.getOwnPropertyNames(window)[Object.getOwnPropertyNames(window).indexOf('URIError') + 1]` |  |  | "globalThis" | "SuppressedError" |  |  |  |  | – | 
| V8 15.4 reorders the global object: Float16Array, SuppressedError and friends move to their spec positions `Object.getOwnPropertyNames(Math).slice(-2).join(',')` |  |  | "f16round,sumPrecise" | "SQRT1_2,SQRT2" |  |  |  |  | – | 
| V8 15.4 reorders the global object: Float16Array, SuppressedError and friends move to their spec positions `Object.getOwnPropertyNames(DataView.prototype).indexOf('getFloat16') < Object.getOwnPropertyNames(DataView.prototype).indexOf('getFloat32')` |  |  | false | true |  |  |  |  | – | 
| V8 15.4 reorders the global object: Float16Array, SuppressedError and friends move to their spec positions `Object.getOwnPropertyNames(Intl.Locale.prototype).indexOf('variants') < Object.getOwnPropertyNames(Intl.Locale.prototype).indexOf('baseName')` |  |  | false | true |  |  |  |  | – | 
| Promise.try returns the promise it was given `((p) => Promise.try(() => p) === p)(Promise.resolve(1))` |  |  | false | true |  |  |  |  | – | 
| Iterator.prototype.includes `typeof Iterator.prototype.includes` |  |  | "undefined" | "function" |  |  |  |  | – | 
| window.requestResize (responsive iframes) `typeof window.requestResize` |  |  | "undefined" | "function" |  |  |  |  | – | 
| FontFace.prototype.width `'width' in FontFace.prototype` |  |  | false | true |  |  |  |  | – | 
| CSS Typed OM in dedicated workers `new Promise((r) => { const w = new Worker(URL.createObjectURL(new Blob(['postMessage(typeof CSSStyleValue)']))); w.onmessage = (e) => r(e.data); })` |  |  | "undefined" | "function" |  |  |  |  | – | 
| SVGTextPathElement gains the TEXTPATH_SIDETYPE constants `'TEXTPATH_SIDETYPE_LEFT' in SVGTextPathElement` |  |  | false | true |  |  |  |  | – | 
| Property order moved: Document.prototype.activeViewTransition `Object.getOwnPropertyNames(Document.prototype).indexOf('activeViewTransition') < Object.getOwnPropertyNames(Document.prototype).indexOf('fonts')` |  |  | false | true |  |  |  |  | – | 
| document.requestStorageAccessFor and FencedFrameConfig.setSharedStorageContext removed `'requestStorageAccessFor' in document` |  |  | true | false |  |  |  |  | – | 
| document.requestStorageAccessFor and FencedFrameConfig.setSharedStorageContext removed `'setSharedStorageContext' in FencedFrameConfig.prototype` |  |  | true | false |  |  |  |  | – | 
| WebCrypto: ML-KEM, ML-DSA, ChaCha20-Poly1305 and X-Wing, with SubtleCrypto.supports() `typeof SubtleCrypto.supports` |  |  |  | "undefined" | "function" |  |  |  | – | 
| WebCrypto: ML-KEM, ML-DSA, ChaCha20-Poly1305 and X-Wing, with SubtleCrypto.supports() `crypto.subtle.generateKey('ChaCha20-Poly1305', false, ['encrypt']).then((k) => k.algorithm.name, (e) => e.name)` |  |  |  | "NotSupportedError" | "ChaCha20-Poly1305" |  |  |  | – | 
| WebCrypto: ML-KEM, ML-DSA, ChaCha20-Poly1305 and X-Wing, with SubtleCrypto.supports() `Object.getOwnPropertyNames(SubtleCrypto.prototype).length` |  |  |  | 13 | 18 |  |  |  | – | 
| Three more permissions-policy features, and Digital Credentials issuance `['digital-credentials-create', 'media-playback-while-not-visible', 'speaker-selection'].map((f) => document.featurePolicy.features().includes(f)).join()` |  |  |  | "false,false,false" | "true,true,true" |  |  |  | – | 
| Three more permissions-policy features, and Digital Credentials issuance `(() => { let read = false; navigator.credentials.create({ get digital() { read = true; } }).catch(() => {}); return read; })()` |  |  |  | false | true |  |  |  | – | 
| IntersectionObserver margins accept absolute length units `(() => { try { return new IntersectionObserver(() => {}, { rootMargin: '1in' }).rootMargin; } catch (e) { return e.name; } })()` |  |  |  | "SyntaxError" | "96px 96px 96px 96px" |  |  |  | – | 
| fetch() of a blob: URL rejects an invalid Range header `fetch(URL.createObjectURL(new Blob(['abc'])), { headers: { Range: 'bytes=x' } }).then((r) => r.status, (e) => e.name)` |  |  |  | 200 | "TypeError" |  |  |  | n/a | 
| SVGAElement.referrerPolicy only reflects known values `(() => { const a = document.createElementNS('http://www.w3.org/2000/svg', 'a'); a.setAttribute('referrerpolicy', 'bogus'); return a.referrerPolicy; })()` |  |  |  | "bogus" | "" |  |  |  | – | 
| RTCDTMFToneChangeEvent no longer needs an init dictionary `RTCDTMFToneChangeEvent.length` |  |  |  | 2 | 1 |  |  |  | – | 
| document.<name> finds object and embed elements that are not 'exposed' `(() => { const o = document.body.appendChild(document.createElement('object')); o.name = 'cc155probe'; o.appendChild(document.createElement('embed')); const r = document.cc155probe === o; o.remove(); return r; })()` |  |  |  | false | true |  |  |  | – | 
| Empty SVG number, integer, angle and length attributes fall back to the initial value `(() => { const t = document.createElementNS('http://www.w3.org/2000/svg', 'feTurbulence'); t.setAttribute('numOctaves', ''); return t.numOctaves.baseVal; })()` |  |  |  | 0 | 1 |  |  |  | – | 
| fetchLater() accepts an empty body `(() => { const c = new AbortController(); try { fetchLater(location.href, { method: 'POST', body: '', signal: c.signal }); return 'ok'; } catch (e) { return e.name; } finally { c.abort(); } })()` |  |  |  | "TypeError" | "ok" |  |  |  | – | 
| <body> margin attributes parse as whole pixels `new Promise((r) => { const f = document.createElement('iframe'); f.srcdoc = '<body marginwidth="10.5">'; f.onload = () => { r(getComputedStyle(f.contentDocument.body).marginLeft); f.remove(); }; document.body.appendChild(f); })` |  |  |  | "10.5px" | "10px" |  |  |  | – | 
| Up to 8 gamepads instead of 4 `navigator.getGamepads().length` |  |  |  | 4 | 8 |  |  |  | – | 
| Soft Navigations interfaces move to their regular place in window `Object.getOwnPropertyNames(window)[Object.getOwnPropertyNames(window).indexOf('PerformanceTiming') + 1]` |  |  |  | "PerformanceServerTiming" | "PerformanceSoftNavigation" |  |  |  | – | 
| Soft Navigations interfaces move to their regular place in window `Object.getOwnPropertyNames(window)[Object.getOwnPropertyNames(window).indexOf('InterestEvent') + 1]` |  |  |  | "InputEvent" | "InteractionContentfulPaint" |  |  |  | – | 
| Soft Navigations interfaces move to their regular place in window `Object.getOwnPropertyNames(PerformanceEntry.prototype).indexOf('navigationId') < Object.getOwnPropertyNames(PerformanceEntry.prototype).indexOf('toJSON')` |  |  |  | false | true |  |  |  | – | 
| WebRTC Diagnostic Logging: three new static methods on RTCPeerConnection `typeof RTCPeerConnection.startDiagnosticLogging` |  |  |  |  | "undefined" | "function" |  |  | n/a | 
| WebRTC Diagnostic Logging: three new static methods on RTCPeerConnection `Object.getOwnPropertyNames(RTCPeerConnection).length` |  |  |  |  | 4 | 7 |  |  | n/a | 
| import ... with { type: 'text' } ships `import('data:text/plain,cc156', { with: { type: 'text' } }).then((m) => m.default, (e) => e.name)` |  |  |  |  | "TypeError" | "cc156" |  |  | n/a | 
| Six secure-context interfaces move together in window's property order `Object.getOwnPropertyNames(window)[Object.getOwnPropertyNames(window).indexOf('Clipboard') + 1]` |  |  |  |  | "ClipboardChangeEvent" | "ClipboardItem" |  |  | – | 
| Six secure-context interfaces move together in window's property order `Object.getOwnPropertyNames(window)[Object.getOwnPropertyNames(window).indexOf('ServiceWorkerContainer') + 1]` |  |  |  |  | "NotRestoredReasonDetails" | "CrashReportContext" |  |  | – | 
| Date.prototype.getTimezoneOffset() keeps seconds for local mean time `Number.isInteger(new Date(Date.UTC(1653, 1, 10, 6, 13, 20)).getTimezoneOffset())` |  |  |  |  | true | false |  |  | n/a | 
| postMessage() options: delegate moves to every port and worker `(() => { let read = false; new MessageChannel().port1.postMessage(0, { get delegate() { read = true; } }); return read; })()` |  |  |  |  | false | true |  |  | – | 
| postMessage() options: delegate moves to every port and worker `(() => { const keys = []; window.postMessage(0, new Proxy({}, { get: (t, k) => { keys.push(String(k)); } })); return keys.join(); })()` |  |  |  |  | "transfer,includeUserActivation,delegate,targetOrigin" | "transfer,delegate,includeUserActivation,targetOrigin" |  |  | – | 
| CanvasRenderingContext2D.direction reports inherit `document.createElement('canvas').getContext('2d').direction` |  |  |  |  | "ltr" | "inherit" |  |  | – | 
| pipeTo() marks the source stream as disturbed at once `(() => { const r = new Response('x'); r.body.pipeTo(new WritableStream({}, { highWaterMark: 0 })); return r.bodyUsed; })()` |  |  |  |  | false | true |  |  | – | 
| RegExp: case folding in /iu backreferences, lookbehind alternatives and legacy /i factoring `/^Ā(..)\1$/iu.test('ĀßSsß')` |  |  |  |  | true | false |  |  | – | 
| RegExp: case folding in /iu backreferences, lookbehind alternatives and legacy /i factoring `/(?<=(ba\|a\|c))x/.exec('bax')[1]` |  |  |  |  | "a" | "ba" |  |  | – | 
| RegExp: case folding in /iu backreferences, lookbehind alternatives and legacy /i factoring `/^(?:ſa\|sb\|ſc\|sd)$/i.test('sb')` |  |  |  |  | false | true |  |  | – | 
| CDATA at the start of an SVG or MathML fragment is parsed as text `(() => { const p = document.createElementNS('http://www.w3.org/2000/svg', 'path'); p.innerHTML = '<![CDATA[y]]>'; return p.textContent; })()` |  |  |  |  | "" | "y" |  |  | – | 
| Class fields named with an escaped get or set parse `(() => { try { return Object.keys(new (eval('(class { g\\u0065t\n a; })'))()).join(); } catch (e) { return e.name; } })()` |  |  |  |  | "SyntaxError" | "get,a" |  |  | n/a | 
| Protected Audience (interest-group and ad-auction) methods removed from navigator `typeof navigator.joinAdInterestGroup` |  |  |  |  |  | "function" | "undefined" |  | n/a | 
| Protected Audience (interest-group and ad-auction) methods removed from navigator `'runAdAuction' in navigator` |  |  |  |  |  | true | false |  | n/a | 
| HTML-in-canvas: drawElementImage, ElementImage and canvas paint events `typeof CanvasRenderingContext2D.prototype.drawElementImage` |  |  |  |  |  | "undefined" | "function" |  | – | 
| HTML-in-canvas: drawElementImage, ElementImage and canvas paint events `typeof ElementImage` |  |  |  |  |  | "undefined" | "function" |  | – | 
| HTML-in-canvas: drawElementImage, ElementImage and canvas paint events `'onpaint' in HTMLCanvasElement.prototype` |  |  |  |  |  | false | true |  | – | 
| New HTML insertion and streaming methods, and Trusted Types parser options `typeof Element.prototype.appendHTML` |  |  |  |  |  | "undefined" | "function" |  | – | 
| New HTML insertion and streaming methods, and Trusted Types parser options `typeof Element.prototype.streamHTML` |  |  |  |  |  | "undefined" | "function" |  | – | 
| New HTML insertion and streaming methods, and Trusted Types parser options `typeof TrustedTypePolicy.prototype.createParserOptions` |  |  |  |  |  | "undefined" | "function" |  | – | 
| window's property order moves again `Object.getOwnPropertyNames(window).indexOf('ProgressEvent') > Object.getOwnPropertyNames(window).indexOf('Lock')` |  |  |  |  |  | false | true |  | – | 
| srgb-linear and display-p3-linear color spaces `(() => { try { return new ImageData(1, 1, { colorSpace: 'srgb-linear' }).colorSpace; } catch (e) { return e.name; } })()` |  |  |  |  |  | "TypeError" | "srgb-linear" |  | – | 
| WebTransport request headers, responseHeaders and reliability `'responseHeaders' in (globalThis.WebTransport?.prototype ?? {})` |  |  |  |  |  | false | true |  | – | 
| WebTransport request headers, responseHeaders and reliability `typeof globalThis.WebTransport?.supportsReliableOnly` |  |  |  |  |  | "undefined" | "boolean" |  | – | 
| Sanitizer setters return booleans `typeof new Sanitizer().removeUnsafe()` |  |  |  |  |  | "undefined" | "boolean" |  | – | 
| HTML parser ignores a head tag inside noscript `new DOMParser().parseFromString('<head><noscript><head><style></style>', 'text/html').querySelector('noscript').childElementCount` |  |  |  |  |  | 0 | 1 |  | – | 
| Uint8Array.setFromHex writes the valid prefix before throwing `(() => { const u = new Uint8Array(16); try { u.setFromHex('aa'.repeat(4) + 'zz' + 'aa'.repeat(11)); } catch (e) {} return u[0]; })()` |  |  |  |  |  | 0 | 170 |  | – | 
| RegExp: class strings fold case under the v and i flags `new RegExp('[\\q{a}]', 'vi').test('A')` |  |  |  |  |  | false | true |  | – | 
| JavaScript parser: await in static fields, conditional chains after arrow functions `(() => { try { return (0, eval)('(function () { var await = 7; return class { static x = await; }; })().x'); } catch (e) { return e.name; } })()` |  |  |  |  |  | "SyntaxError" | 7 |  | n/a | 
| JavaScript parser: await in static fields, conditional chains after arrow functions `(() => { try { (0, eval)('0 ? 0 : () => {} ? 1 : 2'); return 'parsed'; } catch (e) { return e.name; } })()` |  |  |  |  |  | "parsed" | "SyntaxError" |  | n/a | 
| XSLT is off by default: XSLTProcessor is gone and XSL stylesheets are not applied `typeof XSLTProcessor` |  |  |  |  |  |  | "function" | "undefined" | – | 
| Web Install API and the <install> element ship on desktop `typeof HTMLInstallElement` |  |  |  |  |  |  | "undefined" | "function" | n/a | 
| Web Install API and the <install> element ship on desktop `typeof navigator.install` |  |  |  |  |  |  | "undefined" | "function" | n/a | 
| Web Install API and the <install> element ship on desktop `'oninstallresult' in window` |  |  |  |  |  |  | false | true | n/a | 
| Two new permissions-policy features: focus-without-user-activation and web-app-installation `document.featurePolicy.features().includes('focus-without-user-activation')` |  |  |  |  |  |  | false | true | – | 
| Two new permissions-policy features: focus-without-user-activation and web-app-installation `document.featurePolicy.features().includes('web-app-installation')` |  |  |  |  |  |  | false | true | n/a | 
| Long Animation Frame entries report style and layout time `'styleDuration' in PerformanceLongAnimationFrameTiming.prototype` |  |  |  |  |  |  | false | true | – | 
| Long Animation Frame entries report style and layout time `'forcedLayoutDuration' in PerformanceScriptTiming.prototype` |  |  |  |  |  |  | false | true | – | 
| Resource timing names the service-worker router source `'workerMatchedRouterSource' in PerformanceResourceTiming.prototype` |  |  |  |  |  |  | false | true | – | 
| Resource timing names the service-worker router source `'workerFinalRouterSource' in PerformanceResourceTiming.prototype` |  |  |  |  |  |  | false | true | – | 
| SVG textPath side attribute `'side' in SVGTextPathElement.prototype` |  |  |  |  |  |  | false | true | – | 
| WebTransport draining and anticipated concurrent incoming streams `'draining' in WebTransport.prototype` |  |  |  |  |  |  | false | true | – | 
| WebTransport draining and anticipated concurrent incoming streams `'anticipatedConcurrentIncomingBidirectionalStreams' in WebTransport.prototype` |  |  |  |  |  |  | false | true | – | 
| V8 error messages fill in their arguments: Atomics, Intl and Object.fromEntries `(() => { try { Atomics.load(new Int32Array(4), 10); } catch (e) { return e.message; } })()` |  |  |  |  |  |  | "Invalid atomic access index" | "Invalid atomic access index: 10" | – | 
| V8 error messages fill in their arguments: Atomics, Intl and Object.fromEntries `(() => { try { Intl.NumberFormat.prototype.resolvedOptions.call({}); } catch (e) { return e.message; } })()` |  |  |  |  |  |  | "Method UnwrapNumberFormat called on incompatible receiver undefined" | "Method UnwrapNumberFormat called on incompatible receiver #<Object>" | – | 
| V8 error messages fill in their arguments: Atomics, Intl and Object.fromEntries `(() => { try { Object.fromEntries(null); } catch (e) { return e.message; } })()` |  |  |  |  |  |  | "undefined is not iterable" | "null is not iterable" | – | 
| Assigning to a call in strict code is an early SyntaxError `(() => { try { new Function('"use strict"; if (0) f() = 1;'); return 'ok'; } catch (e) { return e.name; } })()` |  |  |  |  |  |  | "ok" | "SyntaxError" | – | 
| RegExp: \p{ASCII} with the v and i flags matches the Kelvin sign and the long s `new RegExp('\\p{ASCII}', 'vi').test('K')` |  |  |  |  |  |  | false | true | – | 
| Parsing and rendering |  |  |  |  |  |  |  |  |  | 
| HTML parser: a truncated <! markup declaration becomes a comment instead of eating the rest `new DOMParser().parseFromString('a<!d>b', 'text/html').body.innerHTML` | "a" | "a<!--d-->b" |  |  |  |  |  |  | – | 
| Processing instructions keep their case `document.createRange().createContextualFragment('<?Foo bar?>').firstChild.target` | "foo" | "Foo" |  |  |  |  |  |  | – | 
| Processing instructions keep their case `document.createProcessingInstruction('x', 'A="1"').getAttribute('A')` | null | "1" |  |  |  |  |  |  | – | 
| URL parser keeps newlines in DATA: URLs regardless of case `new URL('DATA:,a' + String.fromCharCode(10) + 'b').href` | "data:,ab" | "data:,a%0Ab" |  |  |  |  |  |  | – | 
| requestAnimationFrame IDs are no longer consumed by internal callbacks `[requestAnimationFrame(() => {})].map((a) => { const m = document.createElement('marquee'); m.start(); m.stop(); return requestAnimationFrame(() => {}) - a; })[0]` | 2 | 1 |  |  |  |  |  |  | – | 
| createImageBitmap resolves one microtask later, and a zero resize has its own error `(async () => { const o = []; createImageBitmap(new ImageData(1, 1)).then(() => o.push(1)); Promise.resolve().then(() => o.push(2)); await new Promise((r) => setTimeout(r, 50)); return o.join(); })()` | "1,2" | "2,1" |  |  |  |  |  |  | – | 
| createImageBitmap resolves one microtask later, and a zero resize has its own error `createImageBitmap(new ImageData(1, 1), { resizeWidth: 0 }).then(() => 'ok', (e) => e.message.includes('resizeWidth'))` | false | true |  |  |  |  |  |  | – | 
| Disabling a focused form control no longer blurs it synchronously `((i) => { document.body.append(i); i.focus(); i.disabled = true; const r = document.activeElement === i; i.remove(); return r; })(document.createElement('input'))` | false | true |  |  |  |  |  |  | – | 
| XML parsing moved to a Rust parser, and its error messages changed `new DOMParser().parseFromString('<a><b></a>', 'application/xml').querySelector('parsererror div').textContent.trim()` |  | "error on line 1 at column 11: Opening and ending tag mismatch: b line 1 and a" | "error on line 1 at column 10: Unexpected closing tag: a != b" | "error on line 1 at column 11: Opening and ending tag mismatch: b line 1 and a" | "error on line 1 at column 11: Opening and ending tag mismatch: b line 1 and a" | "error on line 1 at column 10: Unexpected closing tag: a != b" |  |  | – | 
| Canvas 2D no longer draws a zero-length stroke `(() => { const c = document.createElement('canvas'); c.width = c.height = 20; const x = c.getContext('2d'); x.lineWidth = 10; x.lineCap = 'round'; x.beginPath(); x.moveTo(10, 10); x.lineTo(10, 10); x.stroke(); return x.getImageData(10, 10, 1, 1).data[3]; })()` |  | 255 | 0 |  |  |  |  |  | – | 
| XSLT output carries a deprecation banner `/This site uses XSLT/.test(new XMLSerializer().serializeToString(((p) => (p.importStylesheet(new DOMParser().parseFromString('<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"><xsl:template match="/"><html><body><p><xsl:value-of select="r/v"/></p></body></html></xsl:template></xsl:stylesheet>', 'application/xml')), p.transformToDocument(new DOMParser().parseFromString('<r><v>1</v></r>', 'application/xml'))))(new XSLTProcessor())))` |  |  | false | true | true | false |  |  | n/a | 
| HarfBuzz 14.4: U+180F is treated as an invisible character `(() => { const x = document.createElement('canvas').getContext('2d'); x.font = '20px sans-serif'; return x.measureText('a' + String.fromCharCode(6159) + 'b').width === x.measureText('ab').width; })()` |  |  |  | false | true |  |  |  | n/a | 
| text-transform: capitalize after a supplementary character `(() => { const p = document.body.appendChild(document.createElement('p')); p.style.textTransform = 'capitalize'; p.innerHTML = '𝐀<span>bc def</span>'; const t = p.innerText; p.remove(); return t; })()` |  |  |  |  | "𝐀Bc Def" | "𝐀bc Def" |  |  | – | 
| Text inputs measure clientWidth and clientLeft from the content box ``(() => { const i = document.body.appendChild(document.createElement('input')); i.style.cssText = 'appearance: none; box-sizing: content-box; width: 100px; border: 5px solid; padding: 0 20px'; const v = `${i.clientWidth},${i.clientLeft}`; i.remove(); return v; })()`` |  |  |  |  |  | "140,5" | "100,25" |  | – | 
| unload handlers stop running by default: the unload permissions policy defaults to none `document.featurePolicy.allowsFeature('unload')` |  |  |  |  |  |  | true | false | n/a | 
| NodeIterator moves its reference correctly when the reference subtree is removed `(() => { const r = document.createElement('div'); r.innerHTML = '<a></a><b><i></i></b>'; const it = document.createNodeIterator(r); it.nextNode(); it.nextNode(); it.nextNode(); it.previousNode(); r.lastChild.remove(); return it.referenceNode.nodeName + ',' + it.pointerBeforeReferenceNode; })()` |  |  |  |  |  |  | "B,true" | "A,false" | – | 
| elementsFromPoint() includes inline ancestors of positioned inline children `(() => { const d = document.body.appendChild(document.createElement('div')); d.style.cssText = 'position:fixed;left:0;top:0;z-index:2147483647'; d.innerHTML = '<span id="cc158a"><span style="position:relative">Text</span></span>'; const r = d.querySelector('[style]').getBoundingClientRect(); const hit = document.elementsFromPoint(r.left + 1, r.top + r.height / 2).some((e) => e.id === 'cc158a'); d.remove(); return hit; })()` |  |  |  |  |  |  | false | true | – | 
| CSS |  |  |  |  |  |  |  |  |  | 
| New CSS property: window-drag `CSS.supports('window-drag', 'move')` | false | true |  |  |  |  |  |  | – | 
| New CSS property: window-drag `'windowDrag' in document.body.style` | false | true |  |  |  |  |  |  | – | 
| corner-shape computed values always serialise as superellipse() `getComputedStyle(document.documentElement).cornerShape` | "round" | "superellipse(1)" |  |  |  |  |  |  | – | 
| CSS relative alpha colours: alpha() `CSS.supports('color', 'alpha(from red / 50%)')` | false | true |  |  |  |  |  |  | – | 
| CSS serialisation: background-size keeps its implicit auto, calc() division reads a / b `Object.assign(document.createElement('div').style, { backgroundSize: '10px' }).backgroundSize` | "10px" | "10px auto" |  |  |  |  |  |  | – | 
| CSS serialisation: background-size keeps its implicit auto, calc() division reads a / b `Object.assign(document.createElement('div').style, { width: 'calc(1px / sibling-index())' }).width` | "calc(1px * (1 / sibling-index()))" | "calc(1px / sibling-index())" |  |  |  |  |  |  | – | 
| Attribute selectors match SVG attribute values case-sensitively `document.createRange().createContextualFragment('<svg type=A>').firstChild.matches('[type=a]')` | true | false |  |  |  |  |  |  | – | 
| CSS Typed OM accepts percentages for opacity and clamps out-of-range computed values `new Promise((r) => r(document.createElement('div').attributeStyleMap.set('opacity', CSS.percent(50)))).then(() => 'ok', (e) => e.name)` | "TypeError" | "ok" |  |  |  |  |  |  | – | 
| Element.pseudo() accepts ::backdrop and ::view-transition `document.documentElement.pseudo('::backdrop') !== null` | false | true |  |  |  |  |  |  | – | 
| Element.pseudo() accepts ::backdrop and ::view-transition `document.documentElement.pseudo('::view-transition') !== null` | false | true |  |  |  |  |  |  | – | 
| Smaller parsing changes: negative overflow-clip-margin, SVG currentTranslate is the same object `CSS.supports('overflow-clip-margin', '-1px')` | false | true |  |  |  |  |  |  | – | 
| Smaller parsing changes: negative overflow-clip-margin, SVG currentTranslate is the same object `((s) => s.currentTranslate === s.currentTranslate)(document.createElementNS('http://www.w3.org/2000/svg', 'svg'))` | false | true |  |  |  |  |  |  | – | 
| New CSS property: scroll-axis-lock `CSS.supports('scroll-axis-lock', 'auto')` |  | false | true |  |  |  |  |  | – | 
| scroll-marker-group: the tabs and links modes turned off `CSS.supports('scroll-marker-group', 'after tabs')` |  | true | false | true |  |  |  |  | – | 
| <area> is display: none by default `(() => { const a = document.body.appendChild(document.createElement('area')); const d = getComputedStyle(a).display; a.remove(); return d; })()` |  | "inline" | "none" |  |  |  |  |  | – | 
| color-mix() now serialises the implied percentage `((s) => { s.color = 'color-mix(in oklab, red 30%, blue)'; return s.color; })(document.createElement('i').style)` |  | "color-mix(red 30%, blue)" | "color-mix(red 30%, blue 70%)" |  |  |  |  |  | – | 
| New CSS property: frame-sizing `CSS.supports('frame-sizing', 'content-height')` |  |  | false | true |  |  |  |  | – | 
| Two new CSS properties: margin-trim and text-decoration-skip-spaces `CSS.supports('margin-trim', 'block')` |  |  |  | false | true |  |  |  | – | 
| Two new CSS properties: margin-trim and text-decoration-skip-spaces `CSS.supports('text-decoration-skip-spaces', 'all')` |  |  |  | false | true | false |  |  | – | 
| @media (dynamic-range) with no value now matches `matchMedia('(dynamic-range)').matches` |  |  |  | false | true |  |  |  | – | 
| CSS symbols() for list-style-type `CSS.supports('list-style-type', 'symbols(cyclic "*")')` |  |  |  | false | true |  |  |  | – | 
| CSS alpha() now requires an explicit alpha `CSS.supports('color', 'alpha(from red)')` |  |  |  | true | false |  |  |  | – | 
| color-mix() and palette-mix() accept percentages that add up to zero `CSS.supports('color', 'color-mix(in srgb, red 0%, blue 0%)')` |  |  |  | false | true |  |  |  | – | 
| CSSPerspective.toMatrix() clamps depths below 1px `new CSSPerspective(CSS.px(0.5)).toMatrix().m34` |  |  |  | -2 | -1 |  |  |  | – | 
| ::first-line styles decide the first line's height on their own `(() => { const h = document.body.appendChild(document.createElement('div')); const r = h.attachShadow({ mode: 'open' }); const s = new CSSStyleSheet(); s.replaceSync('p { font: 16px serif; margin: 0 } p::first-line { font-size: 40px; line-height: 10px }'); r.adoptedStyleSheets = [s]; const p = r.appendChild(document.createElement('p')); p.textContent = 'x'; const v = p.getBoundingClientRect().height === 10; h.remove(); return v; })()` |  |  |  | false | true |  |  |  | – | 
| text-decoration-thickness joins transition: all `(() => { const d = document.body.appendChild(document.createElement('div')); d.style.textDecorationThickness = '0px'; getComputedStyle(d).textDecorationThickness; d.style.transition = 'all 10s'; d.style.textDecorationThickness = '10px'; const n = d.getAnimations().length; d.remove(); return n; })()` |  |  |  | 0 | 1 |  |  |  | – | 
| stroke-dasharray animations no longer add to the underlying value `(() => { const s = document.body.appendChild(document.createElementNS('http://www.w3.org/2000/svg', 'svg')); s.style.strokeDasharray = '1'; const a = s.animate({ strokeDasharray: ['2', '2'] }, { duration: 1e3, composite: 'add' }); a.pause(); const v = getComputedStyle(s).strokeDasharray; a.cancel(); s.remove(); return v; })()` |  |  |  | "3px" | "2px" |  |  |  | – | 
| CSS random() function `CSS.supports('width', 'random(1px, 2px)')` |  |  |  |  | false | true |  |  | – | 
| scroll-snap-type: pair `CSS.supports('scroll-snap-type', 'pair mandatory')` |  |  |  |  | false | true |  |  | – | 
| overflow: clip next to a scrolling axis stays clip `(() => { const d = document.body.appendChild(document.createElement('div')); d.style.overflow = 'clip auto'; const v = getComputedStyle(d).overflowX; d.remove(); return v; })()` |  |  |  |  | "hidden" | "clip" |  |  | n/a | 
| The :navigation-source pseudo-class `CSS.supports('selector(:navigation-source)')` |  |  |  |  | false | true |  |  | – | 
| Native radio buttons get a 50% border radius `(() => { const r = document.body.appendChild(document.createElement('input')); r.type = 'radio'; const v = getComputedStyle(r).borderTopLeftRadius; r.remove(); return v; })()` |  |  |  |  | "0px" | "50%" |  |  | – | 
| CSS text-decoration-inset `CSS.supports('text-decoration-inset', '1px')` |  |  |  |  |  | false | true |  | – | 
| CSS text-decoration-inset `getComputedStyle(document.documentElement).length` |  |  |  |  |  | 479 | 480 |  | n/a | 
| CSS text-overflow with a string value `CSS.supports('text-overflow', '"x"')` |  |  |  |  |  | false | true |  | – | 
| position-visibility: anchor-visible and anchor-valid `CSS.supports('position-visibility', 'anchor-visible')` |  |  |  |  |  | false | true |  | – | 
| position-visibility: anchor-visible and anchor-valid `CSS.supports('position-visibility', 'anchor-valid')` |  |  |  |  |  | false | true |  | – | 
| position-visibility: anchor-visible and anchor-valid `(() => { const d = document.createElement('div'); d.style.positionVisibility = 'anchors-visible'; return d.style.positionVisibility; })()` |  |  |  |  |  | "anchors-visible" | "anchor-visible" |  | – | 
| appearance no longer changes display on non-widget elements `(() => { const s = document.body.appendChild(document.createElement('span')); s.style.appearance = 'auto'; const v = getComputedStyle(s).display; s.remove(); return v; })()` |  |  |  |  |  | "inline-block" | "inline" |  | – | 
| CSS media-state pseudo-classes: :playing, :paused, :seeking, :buffering, :stalled, :muted, :volume-locked `CSS.supports('selector(:playing)')` |  |  |  |  |  |  | false | true | – | 
| CSS ::highlight(*) matches every custom highlight `CSS.supports('selector(::highlight(*))')` |  |  |  |  |  |  | false | true | – | 
| CSS flow-start and flow-end alignment keywords `CSS.supports('align-self', 'flow-start')` |  |  |  |  |  |  | false | true | – | 
| Web Animations: an easing on a keyframe without the property no longer applies to it `(() => { const e = document.body.appendChild(document.createElement('div')); e.style.left = '100px'; const a = e.animate([{ easing: 'steps(2, start)' }, { left: '200px' }], 1e3); a.pause(); a.currentTime = 0; const v = getComputedStyle(e).left; e.remove(); return v; })()` |  |  |  |  |  |  | "150px" | "100px" | – | 
| Media and GPU |  |  |  |  |  |  |  |  |  | 
| Web Audio FFT moved to a Rust library: oscillator output changes in the last bits `((c) => { const o = c.createOscillator(); o.type = 'triangle'; o.frequency.value = 1e4; const k = c.createDynamicsCompressor(); k.threshold.value = -50; k.knee.value = 40; k.ratio.value = 12; k.attack.value = 0; k.release.value = 0.25; o.connect(k); k.connect(c.destination); o.start(0); return c.startRendering().then((b) => b.getChannelData(0).slice(4500, 5e3).reduce((s, x) => s + Math.abs(x), 0)); })(new OfflineAudioContext(1, 44100, 44100))` | 124.04347527516074 | 124.04347776696522 |  |  |  |  |  |  | n/a | 
| IIRFilterNode normalises its coefficients in double precision `(() => { const m = new Float32Array(1); new IIRFilterNode(new OfflineAudioContext(1, 1, 44100), { feedforward: [1], feedback: [1.1, -1.0999] }).getFrequencyResponse(new Float32Array(1), m, new Float32Array(1)); return m[0]; })()` | 9997.6162109375 | 10000 |  |  |  |  |  |  | – | 
| AudioBufferSourceNode honours loopStart when loopEnd is 0 `(async () => { const c = new OfflineAudioContext(1, 8, 8192), b = c.createBuffer(1, 4, 8192); b.getChannelData(0).set([1, 2, 3, 4]); const s = new AudioBufferSourceNode(c, { buffer: b, loop: true, loopStart: 2 / 8192 }); s.connect(c.destination); s.start(); return [...(await c.startRendering()).getChannelData(0)]; })()` | [1,2,3,4,1,2,3,4] | [1,2,3,4,3,4,3,4] |  |  |  |  |  |  | – | 
| getDisplayMedia() reads a new audioSelection option `(() => { let r = false; navigator.mediaDevices.getDisplayMedia({ get audioSelection() { r = true; } }).catch(() => {}); return r; })()` | false | true |  |  |  |  |  |  | – | 
| RTCIceCandidate parses tls candidates that carry a fingerprint `new RTCIceCandidate({ candidate: 'candidate:1 1 tls 1 1.2.3.4 443 typ host fingerprint sha-256;AA', sdpMid: '0' }).address` | null | "1.2.3.4" |  |  |  |  |  |  | – | 
| WebGL: stricter texSubImage2D type validation and array attribute locations (ANGLE) `(() => { const g = document.createElement('canvas').getContext('webgl2'); g.bindTexture(g.TEXTURE_2D, g.createTexture()); g.texImage2D(g.TEXTURE_2D, 0, g.RGBA, 1, 1, 0, g.RGBA, g.UNSIGNED_BYTE, null); g.texSubImage2D(g.TEXTURE_2D, 0, 0, 0, 1, 1, g.RGBA, g.UNSIGNED_SHORT_4_4_4_4, new Uint16Array(1)); return g.getError(); })()` | 0 | 1282 |  |  |  |  |  |  | – | 
| Software WebGL 2 (SwiftShader) reports 8 draw buffers instead of 6 `(() => { const g = document.createElement('canvas').getContext('webgl2'); return g.getParameter(g.MAX_DRAW_BUFFERS); })()` | 6 | 8 |  |  |  |  |  |  | n/a | 
| WebGPU: subgroup-size-control ships, two feature names are recognised `navigator.gpu.requestAdapter().then((a) => !!a && a.features.has('subgroup-size-control'))` | false | true |  |  |  |  |  |  | n/a | 
| IAMF immersive audio in MP4 `document.createElement('video').canPlayType('audio/mp4; codecs="iamf"')` |  | "" | "probably" |  |  |  |  |  | – | 
| IAMF immersive audio in MP4 `MediaSource.isTypeSupported('audio/mp4; codecs="iamf"')` |  | false | true |  |  |  |  |  | – | 
| New WGSL language features: buffer_view and texture_formats_tier1 `navigator.gpu.wgslLanguageFeatures.has('buffer_view')` |  | false | true |  |  |  |  |  | – | 
| New WGSL language features: buffer_view and texture_formats_tier1 `navigator.gpu.wgslLanguageFeatures.has('texture_formats_tier1')` |  | false | true |  |  |  |  |  | – | 
| WebGL shader and program info logs no longer end in a NUL character `(() => { const gl = document.createElement('canvas').getContext('webgl'); const s = gl.createShader(gl.FRAGMENT_SHADER); gl.shaderSource(s, 'void main(){ undeclared_x; }'); gl.compileShader(s); const l = gl.getShaderInfoLog(s); return l.charCodeAt(l.length - 1) === 0; })()` |  | true | false |  |  |  |  |  | – | 
| New WGSL language feature: swizzle_assignment `navigator.gpu.wgslLanguageFeatures.has('swizzle_assignment')` |  |  | false | true |  |  |  |  | – | 
| Web Audio loops short buffers without repeating a sample `(async () => { const c = new OfflineAudioContext(1, 16, 48e3); const b = c.createBuffer(1, 7, 48e3); b.getChannelData(0).set([1, 2, 3, 4, 5, 6, 7]); const s = c.createBufferSource(); s.buffer = b; s.loop = true; s.connect(c.destination); s.start(); const r = await c.startRendering(); return Array.from(r.getChannelData(0)).join(); })()` |  |  |  | "1,2,3,4,5,6,7,7,2,3,4,5,6,7,8,2" | "1,2,3,4,5,6,7,1,2,3,4,5,6,7,1,2" |  |  |  | – | 
| WebGPU vertex format snorm10-10-10-2 works without a flag `navigator.gpu.requestAdapter().then((a) => a.requestDevice()).then(async (d) => { d.pushErrorScope('validation'); const m = d.createShaderModule({ code: '@vertex fn v(@location(0) p: vec4f) -> @builtin(position) vec4f { return p; } @fragment fn f() -> @location(0) vec4f { return vec4f(1); }' }); d.createRenderPipeline({ layout: 'auto', vertex: { module: m, buffers: [{ arrayStride: 4, attributes: [{ format: 'snorm10-10-10-2', offset: 0, shaderLocation: 0 }] }] }, fragment: { module: m, targets: [{ format: 'rgba8unorm' }] } }); const e = await d.popErrorScope(); d.destroy(); return e ? e.constructor.name : 'ok'; })` |  |  |  | "GPUValidationError" | "ok" |  |  |  | n/a | 
| AudioDecoder.isConfigSupported() no longer requires a description `AudioDecoder.isConfigSupported({ codec: 'flac', sampleRate: 48e3, numberOfChannels: 2 }).then((r) => r.supported, (e) => e.name)` |  |  |  |  | "TypeError" | true |  |  | – | 
| OfflineAudioContext gains close() and incremental rendering `'close' in OfflineAudioContext.prototype` |  |  |  |  |  |  | false | true | – | 
| WebGL 2 exposes WEBGL_shader_pixel_local_storage `document.createElement('canvas').getContext('webgl2').getSupportedExtensions().includes('WEBGL_shader_pixel_local_storage')` |  |  |  |  |  |  | false | true | n/a | 
| Automation |  |  |  |  |  |  |  |  |  | 
| DevTools no longer runs Proxy traps in a logged object's prototype chain `(() => { let hit = false; console.log(Object.create(new Proxy({}, { ownKeys() { hit = true; return []; } }))); return hit; })()` | true | false |  |  |  |  |  |  | n/a | 
| DevTools no longer runs Proxy getters when it serialises a logged object `(() => { let hit = false; const e = new Error('x'); Object.defineProperty(e, 'stack', { configurable: true, get: new Proxy(function () {}, { apply() { hit = true; return ''; } }) }); console.log(e); return hit; })()` |  | true | false |  |  |  |  |  | n/a | 
| Intl and time zones |  |  |  |  |  |  |  |  |  | 
| Time zone data updated to tzdata 2026c `new Intl.DateTimeFormat('en-US', { timeZone: 'Africa/Casablanca', timeZoneName: 'longOffset' }).formatToParts(Date.UTC(2026, 9, 1)).find((p) => p.type === 'timeZoneName').value` |  |  | "GMT+01:00" | "GMT+00:00" |  |  |  |  | – | 
| Time zone data updated to tzdata 2026c `new Intl.DateTimeFormat('en-US', { timeZone: 'America/Vancouver', timeZoneName: 'longOffset' }).formatToParts(Date.UTC(2026, 10, 15)).find((p) => p.type === 'timeZoneName').value` |  |  | "GMT-08:00" | "GMT-07:00" |  |  |  |  | – | 
| Time zone data updated to tzdata 2026c `new Intl.DateTimeFormat('en-US', { timeZone: 'America/Edmonton', timeZoneName: 'longOffset' }).formatToParts(Date.UTC(2026, 10, 15)).find((p) => p.type === 'timeZoneName').value` |  |  | "GMT-07:00" | "GMT-06:00" |  |  |  |  | – | 
| Temporal dates keep the locale's own pattern in Japanese and Chinese `Temporal.PlainDate.from('2026-08-27').toLocaleString('ja-JP', { dateStyle: 'long' })` |  |  |  | "2026/8/27" | "2026年8月27日" |  |  |  | – | 
| Temporal dates keep the locale's own pattern in Japanese and Chinese `((f) => f.format(Temporal.PlainDate.from('2026-08-27')) === f.format(new Date(Date.UTC(2026, 7, 27, 12))))(new Intl.DateTimeFormat('ja-JP', { dateStyle: 'long', timeZone: 'UTC' }))` |  |  |  | false | true |  |  |  | – | 
| Intl.Locale rejects an underscore in the variants option `((f) => { try { return f(); } catch (e) { return e.name; } })(() => new Intl.Locale('en', { variants: 'fonipa_scouse' }).toString())` |  |  |  | "en-fonipa-scouse" | "RangeError" |  |  |  | – | 
| Intl.DateTimeFormat reports the year for week-year patterns `'year' in new Intl.DateTimeFormat('te-u-ca-buddhist', { year: 'numeric', month: 'long' }).resolvedOptions()` |  |  |  | false | true |  |  |  | – | 
| Time-zone data: Inuvik keeps summer time, and the EST5EDT-style zones stand alone `new Intl.DateTimeFormat('en-US', { timeZone: 'America/Inuvik', timeZoneName: 'longOffset' }).formatToParts(Date.UTC(2026, 10, 15)).find((p) => p.type === 'timeZoneName').value` |  |  |  |  | "GMT-07:00" | "GMT-06:00" |  |  | – | 
| Time-zone data: Inuvik keeps summer time, and the EST5EDT-style zones stand alone `new Intl.DateTimeFormat('en-US', { timeZone: 'EST5EDT' }).resolvedOptions().timeZone` |  |  |  |  | "America/New_York" | "EST5EDT" |  |  | – | 
| Time-zone data: Inuvik keeps summer time, and the EST5EDT-style zones stand alone `new Date(Date.UTC(1992, 4, 2, 5, 30)).toLocaleString('en-US', { timeZone: 'America/Bogota' })` |  |  |  |  | "5/2/1992, 12:30:00 AM" | "5/2/1992, 1:30:00 AM" |  |  | – | 
| Time-zone data: Inuvik keeps summer time, and the EST5EDT-style zones stand alone `new Intl.DateTimeFormat('en-US', { timeZone: 'America/Winnipeg', dateStyle: 'medium', timeStyle: 'long' }).format(new Date(Date.UTC(2026, 11, 1, 8)))` |  |  |  |  | "Dec 1, 2026, 2:00:00 AM CST" | "Dec 1, 2026, 3:00:00 AM CDT" |  |  | – | 
| Time-zone data: Inuvik keeps summer time, and the EST5EDT-style zones stand alone `new Intl.DateTimeFormat('en-US', { timeZone: 'Europe/London', timeZoneName: 'long' }).format(new Date(Date.UTC(2026, 0, 15, 12)))` |  |  |  |  | "1/15/2026, GMT+00:00" | "1/15/2026, Greenwich Mean Time" |  |  | – | 
| Temporal formatting keeps the year in Chinese and Korean calendars `Temporal.PlainDate.from('2026-08-27').toLocaleString('zh-CN-u-ca-chinese', { dateStyle: 'long' }) === new Date(Date.UTC(2026, 7, 27)).toLocaleDateString('zh-CN-u-ca-chinese', { dateStyle: 'long', timeZone: 'UTC' })` |  |  |  |  | false | true |  |  | – | 
| Temporal toLocaleString with only era keeps the date fields `Temporal.PlainDate.from('2026-11-03').toLocaleString('en-US', { era: 'short' }).includes('2026')` |  |  |  |  |  | false | true |  | – | 
| Intl.NumberFormat rejects decimal strings and BigInts longer than 999,000 characters `(() => { try { return new Intl.NumberFormat('en').format('1.' + '0'.repeat(1e6)); } catch (e) { return e.name; } })()` |  |  |  |  |  |  | "1" | "TypeError" | – | 

A tinted cell differs from the version before it. Values are as the DevTools console prints them. Checks that need a debugger attached are not run live.

## Chrome version history

Every Chrome version since Chrome 100, with the date it reached stable and its newest build on each platform. For every version before the current one that is its final build, the one to compare against when a browser claims that version. Linux often ends a patch or two behind Windows and macOS.

| Chrome | Stable since | Windows | macOS | Linux | Android | 
|---|---|---|---|---|---|
| Chrome 158current | Nov 17, 2026 | 158.0.8127.37 | 158.0.8127.37 | 158.0.8127.36 | 158.0.8127.43 | 
| Chrome 157 | Nov 3, 2026 | 157.0.8103.81 | 157.0.8103.81 | 157.0.8103.78 | 157.0.8103.80 | 
| Chrome 156 | Oct 20, 2026 | 156.0.8078.82 | 156.0.8078.82 | 156.0.8078.79 | 156.0.8078.81 | 
| Chrome 155 | Oct 6, 2026 | 155.0.8059.86 | 155.0.8059.86 | 155.0.8059.84 | 155.0.8059.88 | 
| Chrome 154 | Sep 22, 2026 | 154.0.8037.100 | 154.0.8037.100 | 154.0.8037.97 | 154.0.8037.127 | 
| Chrome 153 | Sep 8, 2026 | 153.0.8010.55 | 153.0.8010.55 | 153.0.8010.52 | 153.0.8010.53 | 
| Chrome 152 | Aug 25, 2026 | 152.0.7977.85 | 152.0.7977.85 | 152.0.7977.82 | 152.0.7977.84 | 
| Chrome 151 | Jul 28, 2026 | 151.0.7922.176 | 151.0.7922.176 | 151.0.7922.173 | 151.0.7922.175 | 
| Chrome 150 | Jun 30, 2026 | 150.0.7871.189 | 150.0.7871.189 | 150.0.7871.186 | 150.0.7871.189 | 
| Chrome 149 | Jun 2, 2026 | 149.0.7827.201 | 149.0.7827.201 | 149.0.7827.200 | 149.0.7827.201 | 
| Chrome 148 | May 5, 2026 | 148.0.7778.218 | 148.0.7778.217 | 148.0.7778.215 | 148.0.7778.217 | 
| Chrome 147 | Apr 7, 2026 | 147.0.7727.139 | 147.0.7727.139 | 147.0.7727.137 | 147.0.7727.138 | 

## Older versions: Chrome 100 to 146

| Chrome | Stable since | Windows | macOS | Linux | Android | 
|---|---|---|---|---|---|
| Chrome 146 | Mar 10, 2026 | 146.0.7680.180 | 146.0.7680.180 | 146.0.7680.177 | 146.0.7680.178 | 
| Chrome 145 | Feb 10, 2026 | 145.0.7632.162 | 145.0.7632.162 | 145.0.7632.159 | 145.0.7632.161 | 
| Chrome 144 | Jan 13, 2026 | 144.0.7559.135 | 144.0.7559.135 | 144.0.7559.132 | 144.0.7559.133 | 
| Chrome 143 | Dec 2, 2025 | 143.0.7499.194 | 143.0.7499.194 | 143.0.7499.192 | 143.0.7499.194 | 
| Chrome 142 | Oct 28, 2025 | 142.0.7444.177 | 142.0.7444.177 | 142.0.7444.175 | 142.0.7444.173 | 
| Chrome 141 | Sep 30, 2025 | 141.0.7390.125 | 141.0.7390.124 | 141.0.7390.122 | 141.0.7390.123 | 
| Chrome 140 | Sep 2, 2025 | 140.0.7339.210 | 140.0.7339.215 | 140.0.7339.207 | 140.0.7339.208 | 
| Chrome 139 | Aug 5, 2025 | 139.0.7258.157 | 139.0.7258.157 | 139.0.7258.154 | 139.0.7258.160 | 
| Chrome 138 | Jun 24, 2025 | 138.0.7204.185 | 138.0.7204.185 | 138.0.7204.183 | 138.0.7204.180 | 
| Chrome 137 | May 27, 2025 | 137.0.7151.122 | 137.0.7151.122 | 137.0.7151.119 | 137.0.7151.117 | 
| Chrome 136 | Apr 29, 2025 | 136.0.7103.116 | 136.0.7103.116 | 136.0.7103.113 | 136.0.7103.127 | 
| Chrome 135 | Apr 1, 2025 | 135.0.7049.117 | 135.0.7049.117 | 135.0.7049.114 | 135.0.7049.113 | 
| Chrome 134 | Mar 4, 2025 | 134.0.6998.179 | 134.0.6998.167 | 134.0.6998.165 | 134.0.6998.136 | 
| Chrome 133 | Feb 4, 2025 | 133.0.6943.143 | 133.0.6943.143 | 133.0.6943.141 | 133.0.6943.138 | 
| Chrome 132 | Jan 14, 2025 | 132.0.6834.197 | 132.0.6834.162 | 132.0.6834.159 | 132.0.6834.165 | 
| Chrome 131 | Nov 12, 2024 | 131.0.6778.267 | 131.0.6778.267 | 131.0.6778.264 | 131.0.6778.261 | 
| Chrome 130 | Oct 15, 2024 | 130.0.6723.119 | 130.0.6723.119 | 130.0.6723.116 | 130.0.6723.103 | 
| Chrome 129 | Sep 17, 2024 | 129.0.6668.103 | 129.0.6668.103 | 129.0.6668.100 | 129.0.6668.102 | 
| Chrome 128 | Aug 20, 2024 | 128.0.6613.139 | 128.0.6613.139 | 128.0.6613.137 | 128.0.6613.148 | 
| Chrome 127 | Jul 23, 2024 | 127.0.6533.122 | 127.0.6533.122 | 127.0.6533.119 | 127.0.6533.106 | 
| Chrome 126 | Jun 11, 2024 | 126.0.6478.185 | 126.0.6478.185 | 126.0.6478.182 | 126.0.6478.188 | 
| Chrome 125 | May 14, 2024 | 125.0.6422.176 | 125.0.6422.176 | 125.0.6422.141 | 125.0.6422.186 | 
| Chrome 124 | Apr 16, 2024 | 124.0.6367.210 | 124.0.6367.210 | 124.0.6367.207 | 124.0.6367.179 | 
| Chrome 123 | Mar 19, 2024 | 123.0.6312.124 | 123.0.6312.124 | 123.0.6312.122 | 123.0.6312.121 | 
| Chrome 122 | Feb 20, 2024 | 122.0.6261.131 | 122.0.6261.129 | 122.0.6261.128 | 122.0.6261.120 | 
| Chrome 121 | Jan 23, 2024 | 121.0.6167.189 | 121.0.6167.184 | 121.0.6167.184 | 121.0.6167.180 | 
| Chrome 120 | Dec 5, 2023 | 120.0.6099.227 | 120.0.6099.234 | 120.0.6099.224 | 120.0.6099.231 | 
| Chrome 119 | Oct 31, 2023 | 119.0.6045.202 | 119.0.6045.199 | 119.0.6045.199 | 119.0.6045.194 | 
| Chrome 118 | Oct 10, 2023 | 118.0.5993.120 | 118.0.5993.117 | 118.0.5993.117 | 118.0.5993.112 | 
| Chrome 117 | Sep 12, 2023 | 117.0.5938.152 | 117.0.5938.149 | 117.0.5938.149 | 117.0.5938.156 | 
| Chrome 116 | Aug 15, 2023 | 116.0.5845.190 | 116.0.5845.187 | 116.0.5845.187 | 116.0.5845.173 | 
| Chrome 115 | Jul 18, 2023 | 115.0.5790.173 | 115.0.5790.170 | 115.0.5790.170 | 115.0.5790.168 | 
| Chrome 114 | May 30, 2023 | 114.0.5735.200 | 114.0.5735.198 | 114.0.5735.198 | 114.0.5735.197 | 
| Chrome 113 | May 2, 2023 | 113.0.5672.128 | 113.0.5672.126 | 113.0.5672.126 | 113.0.5672.163 | 
| Chrome 112 | Apr 4, 2023 | 112.0.5615.139 | 112.0.5615.137 | 112.0.5615.165 | 112.0.5615.136 | 
| Chrome 111 | Mar 7, 2023 | 111.0.5563.148 | 111.0.5563.146 | 111.0.5563.146 | 111.0.5563.116 | 
| Chrome 110 | Feb 7, 2023 | 110.0.5481.179 | 110.0.5481.177 | 110.0.5481.177 | 110.0.5481.154 | 
| Chrome 109 | Jan 10, 2023 | 109.0.5414.121 | 109.0.5414.119 | 109.0.5414.119 | 109.0.5414.118 | 
| Chrome 108 | Nov 29, 2022 | 108.0.5359.126 | 108.0.5359.124 | 108.0.5359.124 | 108.0.5359.128 | 
| Chrome 107 | Oct 25, 2022 | 107.0.5304.123 | 107.0.5304.121 | 107.0.5304.121 | 107.0.5304.141 | 
| Chrome 106 | Sep 27, 2022 | 106.0.5249.120 | 106.0.5249.119 | 106.0.5249.119 | 106.0.5249.126 | 
| Chrome 105 | Aug 30, 2022 | 105.0.5195.128 | 105.0.5195.125 | 105.0.5195.125 | 105.0.5195.136 | 
| Chrome 104 | Aug 2, 2022 | 104.0.5112.102 | 104.0.5112.101 | 104.0.5112.101 | 104.0.5112.97 | 
| Chrome 103 | Jun 21, 2022 | 103.0.5060.134 | 103.0.5060.134 | 103.0.5060.134 | 103.0.5060.129 | 
| Chrome 102 | May 24, 2022 | 102.0.5005.115 | 102.0.5005.115 | 102.0.5005.115 | 102.0.5005.125 | 
| Chrome 101 | Apr 26, 2022 | 101.0.4951.67 | 101.0.4951.64 | 101.0.4951.64 | 101.0.4951.61 | 
| Chrome 100 | Mar 29, 2022 | 100.0.4896.127 | 100.0.4896.127 | 100.0.4896.127 | 100.0.4896.127 | 

From Chromium Dash's stable release lists, checked every six hours.

## How these pages are made

Every comparison uses the same method, so anyone can repeat it. We run Google Chrome's own stable builds of both versions on one machine, dump what a page can observe and what the server receives, and diff the two. We then find each difference in Chromium's source at the two release tags and link it. We do not measure on Chrome for Testing: its default testing configuration changes what a page and a server see (see what differs). Each build runs on a fresh profile, before it has applied Google's field-trial configuration; where that configuration changes a check on an installed Chrome, the check says so. A change Google announces but the source does not turn on by default is marked as unverified, not listed as fact.

To see how your own browser measures up against these values, run the browser fingerprint test, or read how detection uses them in Research.
