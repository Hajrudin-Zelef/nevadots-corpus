---
id: collect-261121-veille-ia/langages-dev/mojo-1-x-in-2026-ownership-kernels-and-a-compiler-that-opened-a-octagono
title: "Mojo 1.x in 2026: Ownership, Kernels, and a Compiler That Opened â octagono"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-06-24"]
keywords: ["mojo", "compiler", "ownership", "kernels", "comptime", "stdlib", "compilador", "list"]
source: https://octagono.org/blog/mojo/
source_anchor: ""
source_lines: [1, 109]
section: "Langages_Dev"
---
Mojo entered 2026 as a vendor language behind a closed compiler and leaves it as something closer to a normal open project with an unusual owner. Qualcomm announced the acquisition of Modular on 2026-06-24 â ~$3.9B all-stock per most press, with some outlets reporting $3.1â3.9B â and closed it on 2026-07-29. Mojo 1.0.0 shipped on 2026-08-11, the compiler and toolchain went Apache 2.0 with LLVM exceptions on 2026-08-18 at ModCon, and MAX became source-available under a bespoke license on 2026-10-22. The opening came in stages â stdlib in 2024, MAX kernels in 2025, compiler in 2026 â while the product Modular sells got more restricted.

Mojo entrÃ³ a 2026 como lenguaje de proveedor con compilador cerrado y sale como un proyecto abierto normal con dueÃ±o poco habitual. Qualcomm anunciÃ³ la compra de Modular el 2026-06-24 â~$3.9B en acciones segÃºn la mayorÃa de la prensa, con medios que reportan $3.1â3.9Bâ y la cerrÃ³ el 2026-07-29. Mojo 1.0.0 saliÃ³ el 2026-08-11, el compilador pasÃ³ a Apache 2.0 con excepciones de LLVM el 2026-08-18 en ModCon, y MAX quedÃ³ source-available bajo licencia propia el 2026-10-22. La apertura fue por etapas (stdlib 2024, kernels 2025, compilador 2026), mientras el producto comercial se restringÃa.

## 1.0 Is a Promise, Not a Freeze

## 1.0 es una promesa, no una congelaciÃ³n

Read the 1.0 announcement literally. â1.x changes should primarily be additiveâ is a source-stability promise, not an API freeze: breaking changes are permitted, âmanaged with careâ, and stability is tracked per API. The deliberately stable stdlib set is small â `Deinitable`, `Movable`, `Copyable` and `ImplicitlyCopyable` are fully stable; `Array`, `List`, `Span`, `String`, `Bool` and `Optional` are only partial. Container and string signatures remain movable ground, so pin versions. The cadence is quick: 1.1.0 on 2026-09-17, 1.2.0 on 2026-10-22, nightlies like `1.3.0.dev2026102605`, and compiler PRs accepted from 1.1.

Conviene leer el anuncio de 1.0 literalmente. âLos cambios en 1.x deberÃan ser principalmente aditivosâ es una promesa de estabilidad de fuente, no una congelaciÃ³n de API: los cambios rompientes se permiten, âgestionados con cuidadoâ, y la estabilidad se marca por API. El stdlib estable es pequeÃ±o: `Deinitable`, `Movable`, `Copyable` e `ImplicitlyCopyable` del todo; `Array`, `List`, `Span`, `String`, `Bool` y `Optional` solo en parte. Las firmas de contenedores y cadenas siguen mÃ³viles: hay que fijar versiones. El ritmo es rÃ¡pido: 1.1.0 el 2026-09-17, 1.2.0 el 2026-10-22, nightlies como `1.3.0.dev2026102605`, PRs al compilador desde 1.1.

## Ownership, Origins, and Value Semantics

## Ownership, orÃgenes y semÃ¡ntica de valor

Mojoâs differentiator against Triton is not syntax; the language models ownership. Values have single owners, argument conventions state the contract â `ref`, `mut`, `imm`, `var`, `out`, `deinit` â and transfer is explicit through the `a^` sigil, so a move reads as a move at the call site. Lifetimes are expressed as origins. 1.0 unified `Pointer` and `UnsafePointer`, making unsafety per-operation rather than per-type, and added experimental interior origins: `List.append` now invalidates outstanding references to elements at compile time.

El diferencial de Mojo frente a Triton no es la sintaxis: el lenguaje modela el ownership. Un solo dueÃ±o por valor; las convenciones de argumento declaran el contrato â`ref`, `mut`, `imm`, `var`, `out`, `deinit`â y la transferencia es explÃcita con el sigilo `a^`, asÃ que el move se ve en la llamada. Los lifetimes son orÃgenes. La 1.0 unificÃ³ `Pointer` y `UnsafePointer` âinseguridad por operaciÃ³n, no por tipoâ y aÃ±adiÃ³ orÃgenes interiores experimentales: `List.append` invalida ahora las referencias vivas a elementos.

## Comptime Instead of Macros

## Comptime en lugar de macros

Compile-time parameters live in square brackets â `def add_tensors[rank: Int](...)` â with `comptime if`, `comptime for`, `comptime match`, and `where` clauses whose failure message comes from `else "message"`. That replaces most uses of macros or template metaprogramming, and diagnostics point at the constraint rather than the expansion. `fn` is gone: `def` absorbed the strict semantics â non-raising by default, explicit `raises`, typed arguments â after `fn` was deprecated at 1.0.0b1. SIMD stays central: `SIMD[dtype, width]`, `Scalar[dtype]` with `Int` now an alias for `Scalar[DType.int]`.

Los parÃ¡metros de compilaciÃ³n van en corchetes â`def add_tensors[rank: Int](...)`â con `comptime if`, `comptime for`, `comptime match` y clÃ¡usulas `where` cuyo fallo sale de `else "message"`. Eso sustituye casi todos los usos de macros o plantillas, y el diagnÃ³stico apunta a la restricciÃ³n, no a la expansiÃ³n. `fn` desapareciÃ³: `def` absorbiÃ³ la semÃ¡ntica estricta âno lanza por defecto, `raises` explÃcito, argumentos tipadosâ tras deprecarse `fn` en 1.0.0b1. SIMD sigue central: `SIMD[dtype, width]`, `Scalar[dtype]` con `Int` como alias de `Scalar[DType.int]`.

## The Python Bridge Is a Library Call

## El puente con Python es una librerÃa

Interop is why most people try Mojo. CPython is embedded as a library via `Python.import_module`, and Mojo exports as a Python module with C-FFI-level call costs; 1.0 made `PythonObject` operators roughly 12Ã faster on the hot path. There is no automated migration path, which is why ânot a Python replacementâ gets repeated by Modular and its critics alike. The bridge serves hot loops rewritten by hand, not ported codebases.

La interop atrae a la mayorÃa a Mojo. CPython se incrusta como librerÃa vÃa `Python.import_module`, y el cÃ³digo Mojo se exporta como mÃ³dulo de Python con costes de llamada de FFI de C; la 1.0 acelerÃ³ los operadores de `PythonObject` unas 12Ã en la ruta caliente. No hay migraciÃ³n automatizada, y por eso Modular y sus crÃticos repiten que âno es un reemplazo de Pythonâ. El puente sirve para bucles calientes a mano, no para portar cÃ³digo entero.

## One Kernel, Many Vendors

## Un kernel, muchos proveedores

MAX is the serving half: graph compiler, runtime, driver, a Mojo kernel library, a Python modeling API, `max serve` exposing `/v1/chat/completions`, `/v1/responses` and `/v1/audio/speech`, and `max benchmark`. Targets run from NVIDIA Turing `sm_75` to Blackwell `sm_100/103/110/120/121`, AMD CDNA2âCDNA4 including `gfx950` and MI355X, RDNA2âRDNA4, and Apple M1âM5 via Metal for a subset of models kept out of nightly CI. Trainium, TPU, Cloud AI 100 Ultra and Dragonfly are announced, not in production; serving is tested on B200 and MI355X.

MAX es la mitad de serving: compilador de grafos, runtime, driver, librerÃa de kernels Mojo, API de modelado en Python, `max serve` (`/v1/chat/completions`, `/v1/responses`, `/v1/audio/speech`) y `max benchmark`. Los objetivos van de NVIDIA Turing `sm_75` a Blackwell `sm_100/103/110/120/121`, AMD CDNA2âCDNA4 (`gfx950`, MI355X), RDNA2âRDNA4 y Apple M1âM5 vÃa Metal, para un subconjunto de modelos fuera del CI nocturno. Trainium, TPU, Cloud AI 100 Ultra y Dragonfly estÃ¡n anunciados, no en producciÃ³n; el serving se prueba en B200 y MI355X.

Quantization carries the platform: NVFP4 block-scaled on SM100, MXFP8, FP8, int8 W8A8, MXint8 KV-cache with fp16 scales halving KV memory, and int8 requantization of NVFP4 on M5 at about 2.56Ã versus W4A16. Every published comparison is vendor-reported. Against vLLM on B200: 5.5Ã better P50 TTFT on Kimi-K2.5 (239 ms vs 1,303 ms), 2.5Ã better P99 TTFT on Gemma-4-31B-it, 1.5Ã throughput (~625k vs 419k tok/min/GPU on Kimi). Against SGLang on B300 with a 400B+ Hippocratic AI workload: sub-500 ms mean TTFT, ~30% faster P99 end-to-end. FLUX.2-dev: 6.9Ã over PyTorch Diffusers with torch.compile on B200, 3.8Ã on MI355X.

La cuantizaciÃ³n sostiene la plataforma: NVFP4 con escalado por bloques en SM100, MXFP8, FP8, int8 W8A8, KV-cache MXint8 (escalas fp16, mitad de memoria) y recuantizaciÃ³n int8 de NVFP4 en M5, ~2.56Ã frente a W4A16. Todas las cifras son del proveedor. Frente a vLLM en B200: 5.5Ã mejor P50 de TTFT en Kimi-K2.5 (239 vs 1,303 ms), 2.5Ã mejor P99 en Gemma-4-31B-it, 1.5Ã de throughput (~625k vs 419k tok/min/GPU). Frente a SGLang en B300 (400B+, Hippocratic AI): TTFT medio bajo 500 ms, P99 ~30% mÃ¡s rÃ¡pido. FLUX.2-dev: 6.9Ã sobre PyTorch Diffusers con torch.compile en B200, 3.8Ã en MI355X.

Treat those as claims. No independent reproduction has been published; comparisons are single-model and single-SKU, several cite P50-to-P50 TTFT rather than goodput at an SLO, and baseline vLLM/SGLang configurations are undisclosed. The closest third-party signal is indirect: Modular ran OpenRouter traffic as âModelRunâ and claims top-quartile latency and throughput in Artificial Analysis, also not independently re-verified.

Hay que tomarlas como afirmaciones. No hay reproducciÃ³n independiente publicada; las comparaciones son de un modelo y un SKU, varias citan P50 a P50 en vez de goodput con un SLO, y las configuraciones base de vLLM/SGLang no se revelan. La seÃ±al de terceros mÃ¡s cercana es indirecta: Modular sirviÃ³ trÃ¡fico de OpenRouter como âModelRunâ y afirma latencia y throughput del cuartil superior en Artificial Analysis, tampoco reverificado.

## What Is Actually Missing

## Lo que realmente falta

The gap list is longer than release notes suggest. There is no Mojo package registry: packaging runs through `pixi`, `uv` or conda at `conda.modular.com`, with `max[all]`, `max[serve]` and `max[benchmark]` extras, and the `modular` meta-package was retired in 26.6. Traits are nominal with `where` constraints, conditional conformance and negative bounds (`not Trait`) in 1.2, but there are no existentials or dynamic traits. Async is unstable â 1.2 renamed `async`/`await` to `__async`/`__await`. C++ interop, a profiler and native Windows are missing, and multi-type `except` is forbidden.

Los huecos son mÃ¡s de los que sugieren las notas. No hay registro de paquetes Mojo: el empaquetado pasa por `pixi`, `uv` o conda en `conda.modular.com`, con extras `max[all]`, `max[serve]` y `max[benchmark]`; el metapaquete `modular` se retirÃ³ en 26.6. Los traits son nominales, con restricciones `where`, conformancia condicional y cotas negativas (`not Trait`) en 1.2, pero sin existenciales ni traits dinÃ¡micos. Async es inestable: 1.2 renombrÃ³ `async`/`await` a `__async`/`__await`. Faltan interop con C++, profiler y Windows nativo; se prohÃbe `except` de varios tipos.

Then the license. MAX is not open source. Modular 26.7 made it source-available under the MAX License: commercial use without royalties, but modified source may not be published or distributed except as contribution forks, MAX may not be fed to an AI system to produce a reimplementation, and commercial hosted inference must display a âModular Acceleratedâ wordmark. Kernels, Mojo interfaces and models stay Apache 2.0. A license Q&A with Lattner and Davis is set for 2026-11-10. Opening the compiler three weeks after the acquisition closed drew visible rug-pull sentiment on Modularâs own forum.

Luego estÃ¡ la licencia: MAX no es cÃ³digo abierto. Modular 26.7 lo volviÃ³ source-available bajo la MAX License: uso comercial sin regalÃas, pero no puede publicarse ni distribuirse cÃ³digo modificado salvo como fork de contribuciÃ³n, ni alimentar MAX a una IA para reimplementarlo, y la inferencia alojada comercial debe mostrar la marca âModular Acceleratedâ. Kernels, interfaces de Mojo y modelos siguen en Apache 2.0. La sesiÃ³n sobre la licencia con Lattner y Davis queda para el 2026-11-10. Abrir el compilador tres semanas despuÃ©s de la compra dejÃ³ un rug-pull visible en el foro de Modular.

## Where It Sits

## DÃ³nde se ubica

Against CUDA and CUTLASS, Mojo wins on portability and loses on 18 years of cuBLAS, cuDNN and NCCL. Triton owns the pragmatic middle â PyTorch-native, inductor, a huge install base â while Mojoâs edge is being a full language with ownership and traits that also targets CPU and non-NVIDIA silicon. Julia wins on interactivity, packages and scientific breadth; Mojo on AOT binaries and memory control. Rust has the safest systems story and `burn`/`candle`, but no unified accelerator abstraction.

Frente a CUDA y CUTLASS, Mojo gana en portabilidad y pierde ante 18 aÃ±os de cuBLAS, cuDNN y NCCL. Triton domina el tÃ©rmino medio (nativo de PyTorch, inductor, base enorme) mientras Mojo aporta un lenguaje completo con ownership y traits que tambiÃ©n apunta a CPU y silicio no NVIDIA. Julia gana en interactividad, paquetes y amplitud cientÃfica; Mojo, en binarios AOT y memoria. Rust tiene la historia de sistemas mÃ¡s segura y `burn`/`candle`, sin abstracciÃ³n unificada de aceleradores.

Watch three things: the alliance program promised for the end of 2026, the November license Q&A, and whether compiler contributions hold pace once the acquisition novelty fades. Production evidence for Mojo-the-language remains thin: MAX and Modular Cloud have named customers, including MiniMax on M3 at âbillions of tokens/minâ, but public third-party Mojo codebases are mostly Modular, grant cohorts such as `noeira`, `bajo` and a Mojo FFT near cuFFT, plus hackathons.

Tres cosas que observar: el programa de alianza prometido para finales de 2026, la sesiÃ³n sobre la licencia en noviembre, y si las contribuciones al compilador mantienen el ritmo cuando pase la novedad. La evidencia de producciÃ³n de Mojo sigue escasa: MAX y Modular Cloud tienen clientes con nombre, como MiniMax con M3 a âmiles de millones de tokens por minutoâ, pero los codebases pÃºblicos de terceros son sobre todo Modular, grants y hackathons.

## References

## Referencias

- Modular. *Mojo releases index* . mojolang.org/releases
- Modular. *Mojo v1.0.0 changelog* . mojolang.org
- Modular. *Mojo v1.2.0 changelog* . mojolang.org
- Modular. *Modular 26.5: Mojo 1.0 is here!* modular.com
- Modular. *Mojo is now open source* . modular.com
- Modular. *ModCon 2026: Open source, open cloud, open silicon* . modular.com
- Modular. *Modular 26.7: Opening up MAX* . modular.com
- Modular. *MAX is now source available* . modular.com
- Modular. *Three trends from MLSys 2026* . modular.com
- Modular. *MAX GPU compatibility* . max.modular.com
- Modular. *Mojo roadmap* . mojolang.org/docs/roadmap
- Modular. *Mojo manual: value ownership* . mojolang.org/docs/manual/values
- Qualcomm. *Qualcomm completes acquisition of Modular* . qualcomm.com
- Modular. *modular/modular repository* . GitHub

- Modular. *Ãndice de versiones de Mojo* . mojolang.org/releases
- Modular. *Changelog de Mojo v1.0.0* . mojolang.org
- Modular. *Changelog de Mojo v1.2.0* . mojolang.org
- Modular. *Modular 26.5: Â¡Mojo 1.0 ya estÃ¡ aquÃ!* modular.com
- Modular. *Mojo ya es cÃ³digo abierto* . modular.com
- Modular. *ModCon 2026: cÃ³digo abierto, nube abierta, silicio abierto* . modular.com
- Modular. *Modular 26.7: abrir MAX* . modular.com
- Modular. *MAX ya estÃ¡ disponible como cÃ³digo fuente* . modular.com
- Modular. *Tres tendencias de MLSys 2026* . modular.com
- Modular. *Compatibilidad de GPU de MAX* . max.modular.com
- Modular. *Roadmap de Mojo* . mojolang.org/docs/roadmap
- Modular. *Manual de Mojo: ownership de valores* . mojolang.org/docs/manual/values
- Qualcomm. *Qualcomm completa la adquisiciÃ³n de Modular* . qualcomm.com
- Modular. *Repositorio modular/modular* . GitHub
