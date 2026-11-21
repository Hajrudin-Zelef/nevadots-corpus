---
id: collect-261121-veille-ia/mojo-modular/decimo-v0-16-0-and-argmojo-v0-10-0-updated-for-mojo-v1-2-0
title: "Decimo v0.16.0 and ArgMojo v0.10.0, updated for Mojo v1.2.0"
domain: mojo-modular
role: reference
task: article
actors: []
dates: []
keywords: ["decimo", "argmojo", "mojo", "updated", "release", "pixi", "used", "wrapper"]
source: https://forum.modular.com/t/decimo-v0-16-0-and-argmojo-v0-10-0-updated-for-mojo-v1-2-0/3589
source_anchor: ""
source_lines: [1, 31]
section: "Mojo_Modular"
---
Hi Mojicians,

I am glad to let you know that the libraries I am maintaining are updated for Mojo v1.2.0. Both are on the modular-community channel:

```
pixi add decimo
pixi add argmojo
```

In Decimo v0.16.0, `BigFloat` is implemented in pure Mojo now. It used to be a wrapper over GNU MPFR. That wrapper is still in the source as `MPF`, in case you are interested (it needs MPFR installed):

```
from decimo.mpf.mpf import MPF
```

For ArgMojo, thanks to Mojo v1.2.0, `Defaultable` provides a default `__init__` that builds each field by reflection. Because of this, the code that initializes a `Parsable` struct is greatly simplified (hooray). ArgMojo used to do this itself. Now the compiler does it.

Release notes: Decimo v0.16.0, ArgMojo v0.10.0

Relevant posts:

Decimo: v0.14.0, v0.10.0, v0.9.0, v0.8.0, v0.7.0, v0.6.0, v0.2, first release

ArgMojo: v0.8.0, v0.5.0, first release


             
              
                  
                  
                3 Likes
