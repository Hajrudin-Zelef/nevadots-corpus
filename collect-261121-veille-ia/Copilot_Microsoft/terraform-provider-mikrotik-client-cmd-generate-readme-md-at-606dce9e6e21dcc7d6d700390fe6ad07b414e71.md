---
id: collect-261121-veille-ia/copilot-microsoft/terraform-provider-mikrotik-client-cmd-generate-readme-md-at-606dce9e6e21dcc7d6d700390fe6ad07b414e71
title: "terraform-provider-mikrotik/client/cmd/generate/README.md at 606dce9e6e21dcc7d6d700390fe6ad07b414e712 · ddelnano/terraform-provider-mikrotik"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["client", "generate", "terraform-provider-mikrotik", "file", "resource", "test", "tool", "outfile"]
source: https://github.com/ddelnano/terraform-provider-mikrotik/blob/HEAD/client/cmd/generate/README.md
source_anchor: ""
source_lines: [1, 12]
section: "Copilot_Microsoft"
---
This tool allows generating MikroTik resources for API client.

You can generate MikroTik resource and save it to file using remote RouterOS instance (requires valid credentials in environment)

`$ go run ./client/cmd/generate resource -query -basePath /ip/arp -outFile client/arp.go`
or using offline definition file created with inspection tool:

`$ go run ./client/cmd/generate resource -basePath /ip/arp -definitionFile arp.json -outFile client/arp.go`
If you have already generated resource in `arp.go` file, you can also generate a basic test file for it:

`$ go run ./client/cmd/generate test -sourceFile client/arp.go -outFile client/arp_test.go`
carefully review the test file, as codegen tool is still experimental.
