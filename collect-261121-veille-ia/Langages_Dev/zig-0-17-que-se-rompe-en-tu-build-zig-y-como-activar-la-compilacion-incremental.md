---
id: collect-261121-veille-ia/langages-dev/zig-0-17-que-se-rompe-en-tu-build-zig-y-como-activar-la-compilacion-incremental
title: "Zig 0.17: qué se rompe en tu build.zig y cómo activar la compilación incremental"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["build", "compilacion", "zig", "como", "incremental", "puede", "rust", "ahora"]
source: https://www.yodev.dev/t/zig-0-17-que-se-rompe-en-tu-build-zig-y-como-activar-la-compilacion-incremental/5522?u=devy
source_anchor: ""
source_lines: [1, 163]
section: "Langages_Dev"
---
Zig 0.17 ya está disponible: tu `build.zig` necesita cambios, la compilación incremental funciona en x86_64-linux con un solo flag y ZLS todavía no es compatible. Estas tres cosas importan si mantienes un proyecto en Zig. Además, `@bitCast` puede cambiar de comportamiento sin que el compilador te avise. La versión reúne cinco meses de trabajo de 206 contribuidores en 925 commits, y casi todo ese esfuerzo se concentró en el build system y el linker.

## ¿Qué es Zig y en qué se diferencia de Rust?

Zig es un lenguaje de programación de propósito general y un toolchain para programación de sistemas, orientado a software robusto, óptimo y reutilizable. Compite en el mismo terreno que C, C++ y Rust, aunque con una filosofía distinta.

Rust garantiza la seguridad de memoria en tiempo de compilación mediante su *borrow checker*. Zig no tiene *borrow checker*: te da allocators explícitos, ejecución de código en tiempo de compilación con `comptime` y chequeos de seguridad en tiempo de ejecución en los modos de compilación seguros. A cambio, ofrece un lenguaje más pequeño e interoperabilidad directa con C. Su toolchain también compila C y C++ de forma cruzada con `zig cc`. Si te preguntas *zig vs rust*, la elección se reduce a cuánto control manual quieres y cuántas garantías prefieres delegar al compilador.

Zig todavía no llega a la 1.0, así que cada versión menor puede romper código. Por eso cada actualización necesita una guía de migración. El proyecto lo financia la Zig Software Foundation, una organización sin fines de lucro, y su código fuente vive ahora en Codeberg en lugar de GitHub.

## ¿Qué cambia en zig build con Zig 0.17?

`zig build` ya no ejecuta tu `build.zig` en el mismo proceso que corre el build. El antiguo *build runner* se separa en dos piezas:

- El **configurer** ejecuta tu`build.zig` y genera el grafo de build.
- El **maker** se encarga de la gestión de paquetes y ejecuta ese grafo.

En la práctica, esto tiene tres efectos:

- **El maker se compila una sola vez.** Se construye con optimizaciones la primera vez que usas Zig después de instalarlo, y no necesita recompilarse cuando editas`build.zig` .
- **La configuración puede saltarse.** Si no cambió nada relevante,`zig build` puede omitir tu`build.zig` por completo.
- **La configuración ahora es un dato.** El grafo de build se serializa en un formato binario compacto que pueden leer herramientas externas. Para verlo, pasa`--print-configuration` a`zig build` y lo imprimirá como`.zon` en la salida estándar.

Ese último punto es la base del nuevo **Build Server Protocol**. Si pasas `--listen=-`, el build system expone un protocolo que permite a un cliente conectado inspeccionar el grafo de build completo, recibir notificaciones cuando un paso empieza y termina, y pedir que se ejecuten pasos concretos. Está pensado para IDEs y herramientas de terceros. Como consecuencia, ya no es posible reemplazar el *build runner*: ese caso de uso ahora lo cubre el protocolo.

## ¿Qué tienes que cambiar en build.zig?

Las *release notes* listan explícitamente los cambios de API del build system. Estos son los que más probablemente afecten a un proyecto típico.

**Argumentos del paso Run.** Los scripts de build ya no pueden inspeccionar los argumentos que se pasan al programa. A cambio, modificar esos argumentos ya no obliga a recompilar `build.zig`:

```
-if (b.args) |args| {
-    run_cmd.addArgs(args);
-}
+run_cmd.addPassthruArgs();
```

**Rutas del paso Fmt.** Ahora son listas de `LazyPath`, y se crean con `b.pathList`:

```
-    const fmt_include_paths = &.{ "lib", "src", "test", "tools", "build.zig", "build.zig.zon" };
+    const fmt_include_paths = b.pathList(&.{ "lib", "src", "test", "tools", "build.zig", "build.zig.zon" });
```

**Renombres y eliminaciones:**

- `b.build_root` (un Directory) pasa a ser`b.root` (un Path).
- `LazyPath.getDisplayName` pasa a ser`format` , que se usa con`"{f}"` .
- Se elimina `LazyPath.basename` , porque su valor no se conoce hasta la fase de make.
- Los helpers de argumentos del paso `Run` se unifican en una sola variante terminada en`2` . Por ejemplo,`addArtifactArg` y`addPrefixedArtifactArg` pasan a ser`addArtifactArg2` , y`addFileArg` y`addPrefixedFileArg` pasan a ser`addFileArg2` . El mismo patrón aplica a los argumentos de archivo de salida, contenido de archivo, directorio, directorio de salida y*dep-file* .
- `ConfigHeader.Options.include_guard_override` pasa a ser`include_guard` .
- Las opciones que reciben una ruta ahora deben indicar qué tipo de ruta es: `addOptionPath` para un archivo,`addOptionPathDirectory` para un directorio, o`addOptionPathUntracked` para excluirla del seguimiento de dependencias.

**Traducción de C.** `@cImport` queda eliminado; ya estaba deprecado en la 0.16. `std.Build.Step.TranslateC` se depreca a favor del paquete oficial `translate-c`. Primero agrega la dependencia:

```
zig fetch --save git+https://codeberg.org/ziglang/translate-c
```

Luego reemplaza `b.addTranslateC(...)` por el `Translator` del paquete:

```
+const Translator = @import("translate_c").Translator;
 ...
-    const translate_c = b.addTranslateC(.{
-        .root_source_file = b.path("src/c.h"),
+    const translate_c = b.dependency("translate_c", .{});
+
+    const translator: Translator = .init(translate_c, .{
+        .c_source_file = b.path("src/c.h"),
         .target = target,
         .optimize = optimize,
     });
 ...
-                    .module = translate_c.createModule(),
+                    .module = translator.mod,
```

**Efectos secundarios en la configuración.** Zig ahora detecta cuándo tu lógica de configuración hace algo que la caché no puede rastrear. Llamar a `findProgram`, por ejemplo, “envenena” la caché de configuración, y eso obliga a ejecutar `build.zig` en cada build. Si solo necesitas el programa en tiempo de build, usa `findProgramLazy`: devuelve un `LazyPath` y deja la caché intacta. Si tu configuración lee archivos o directorios, declara esa dependencia con `std.Build.dependOnFileContents` o una de sus variantes, y la caché seguirá funcionando.

## ¿Cómo activar la compilación incremental en Zig?

Ejecuta el build con estos dos flags:

```
zig build -fincremental --watch
```

El build system vigila tus archivos fuente y hace una recompilación incremental cada vez que cambian. Según las *release notes*, la *incremental compilation* ya funciona para la mayoría de los proyectos que apuntan a `x86_64-linux`. Es el resultado de muchas correcciones de bugs y del mejor soporte en el nuevo linker ELF que llegó en la versión anterior.

Las *release notes* fijan tres condiciones:

- **Por ahora requiere `--watch`.** Usar compilación incremental sin el modo watch figura como trabajo futuro.
- **En la práctica es solo para Linux x86_64.** El nuevo linker ELF es la pieza que lo hace posible. En Mac y aarch64 hay que esperar un nuevo linker Mach-O y un backend aarch64 propio, ambos planeados para versiones futuras.
- **El nuevo linker sigue desactivado por defecto.** Todavía no alcanza del todo al linker ELF heredado en funcionalidad, pero se activa automáticamente cuando usas compilación incremental, igual que en la 0.16.

Si quieres entender cómo funciona por dentro, las *release notes* recomiendan este artículo de Matthew Lugg, miembro del equipo central de Zig: Inside Zig's Incremental Compilation | mlugg.co.uk

## ¿Funciona ZLS con Zig 0.17?

No: al momento de publicar esta nota (octubre de 2026), ZLS no funciona con Zig 0.17. Las *release notes* indican que la separación entre configurer y maker es un cambio incompatible que impide que ZLS, el *language server* de Zig, funcione con esta versión. Los equipos de Zig y ZLS trabajan juntos para ampliar el Build Server Protocol, de modo que ZLS recupere sus funciones y con el tiempo las supere.

Si tu flujo en el editor depende de ZLS para el autocompletado, la navegación al código o los diagnósticos, no actualices todavía tu entorno principal. Revisa el estado del proyecto en ZLS language server - zigtools antes de cambiar.

## ¿Qué puede romperse sin que el compilador te avise?

`@bitCast` es el cambio más peligroso de la 0.17, porque puede romper código sin generar ningún error de compilación. Ahora se define como la reinterpretación de la representación *lógica* de bits de un valor.

- **Lo que sigue igual:** los casts entre enteros, y entre enteros y`packed struct` o`packed union` .
- **Lo que cambia:** los casts que involucran arrays o vectores tienen una semántica nueva. Las*release notes* advierten que esto puede romper código existente sin errores de compilación. La nueva definición es independiente del*endianness* y coincide en gran medida con el comportamiento anterior en arquitecturas*little-endian* .

Antes de actualizar, busca en tu código las llamadas a `@bitCast` que involucren arrays o vectores y revísalas una por una.

Los casts con `extern struct` o `extern union` ahora son un error de compilación. Si lo que buscas es *type punning*, las *release notes* recomiendan `@ptrCast` o una `extern union`.

## ¿Qué más cambió en el lenguaje?

Los siguientes cambios fallan de forma visible en compilación, así que el compilador te llevará directo a ellos:

- **Se elimina la multiplicación de arrays (`a ** b`).** Usa`@splat` , por ejemplo`var result: [n]u8 = @splat(0);` .
- **Se elimina `void{}`.** Escribe`{}` .
- **Se elimina la captura en `errdefer |err|`.** Las notas sugieren dividir la función en dos y manejar el error con`catch` en la función externa.
- **Se elimina `i0`.** Casi con seguridad puedes reemplazarlo por`u0` .
- **`@backingInt` y `@fromBackingInt` reemplazan a `@intFromEnum` y `@enumFromInt`.**`zig fmt` hace esta actualización automáticamente.
- **`@hasDecl` ahora devuelve `true` solo para declaraciones públicas,** incluso cuando se llama desde el mismo archivo.
- **Nuevos builtins:**`@divCeil` , y`@SpirvType` para targets SPIR-V.

La biblioteca estándar tiene su propia lista de cambios:

- `std.builtin` se depreca a favor de`std.lang` .
- `OptimizeMode` pasa a ser`Optimize` , con los tags`debug` ,`safe` ,`fast` y`small` . Los nombres antiguos siguen existiendo, pero las comparaciones con`==` contra ellos dejan de compilar.
- `StackFallbackAllocator` pasa a llamarse`BufferFirstAllocator` .
- `std.heap.DebugAllocator` se depreca a favor del nuevo`SafeAllocator` , que es*thread-safe* .
- `std.fmt.allocPrint(arena, …)` pasa a ser`arena.print(…)` .

## ¿Conviene actualizar ya?

Depende de tu configuración:

- **Actualiza si** compilas sobre todo para x86_64-linux, no dependes de ZLS y quieres recompilaciones rápidas. La compilación incremental es la recompensa, y los cambios en`build.zig` son mecánicos.
- **Espera si** tu flujo en el editor depende de ZLS, o si usas pasos`Run` con*response files* . Las*release notes* lo listan como una regresión conocida de esta versión.
- **En cualquier caso,** revisa tus`@bitCast` antes de publicar cualquier binario compilado con la 0.17.

La versión también acerca a Zig a la 1.0. El equipo tomó decisiones sobre unas 150 propuestas del lenguaje en este ciclo: aceptó alrededor de 25 y rechazó alrededor de 125. El backend de WebAssembly ya pasa el 100% de las pruebas de comportamiento frente al backend de LLVM, aunque todavía no es el predeterminado en modo debug porque le falta soporte de información de depuración.

Puedes descargar esta versión desde Download ⚡ Zig Programming Language o instalarla con un gestor de paquetes, siguiendo la guía del proyecto.


**¿Y tú?** ¿En qué proyecto vas a probar primero la compilación incremental?

¿Vas a actualizar a Zig 0.17?

- Sí, ya mismo
- Cuando ZLS sea compatible
- Me quedo en la 0.16 por ahora
- Todavía no uso Zig (cuéntanos qué te frena)

 
 
Comenta abajo o, si este artículo te llegó por correo, responde directamente al email: tu respuesta se publica aquí.

Relacionado: Lightpanda: El Headless Browser Hecho para Agentes de IA (No para Humanos)
