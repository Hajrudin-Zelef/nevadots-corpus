---
id: collect-261121-veille-ia/langages-dev/rust-1-98-0-pre-release-testing
title: "Rust 1.98.0 pre-release testing"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-08-18"]
keywords: ["pre-release", "testing", "release", "stable", "rust", "notes", "rustup", "dist"]
source: https://internals.rust-lang.org/t/rust-1-98-0-pre-release-testing/24541
source_anchor: ""
source_lines: [1, 39]
section: "Langages_Dev"
---
The 1.98.0 pre-release is ready for testing. The release is scheduled for
August 20. Release notes can be found here.

You can try it out locally by running:

```
RUSTUP_DIST_SERVER=https://dev-static.rust-lang.org rustup update stable
```

The index is https://dev-static.rust-lang.org/dist/2026-08-18/index.html.

General feedback about our pre-release process is welcome on this GitHub issue.

             
              
                  
                  
                1 Like
              

            
           
          
            
            
              ~~One error in the release notes: they claim to have stabilised `Send` and `Sync` for `std::process::CommandArgs`, but I think actually the opposite occurred (negative impls `!Send` and `!Sync` were stabilised on the type).~~ EDIT: this was wrong, the outdated links to stable catch me out sometimes and I may have muddled it with a different commit that isn't part of the stable release and affects a different type.

             
              
                  
                  
                

            
           
          
            
            
              No, it just links to the docs of stable instead of beta.
