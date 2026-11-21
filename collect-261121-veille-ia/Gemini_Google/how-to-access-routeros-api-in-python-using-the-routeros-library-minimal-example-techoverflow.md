---
id: collect-261121-veille-ia/mikrotik/how-to-access-routeros-api-in-python-using-the-routeros-library-minimal-example-techoverflow
title: "How to access RouterOS API in Python using the 'routeros' library (minimal example) | TechOverflow"
domain: mikrotik
role: reference
task: article
actors: ["Google"]
dates: ["2023-09-16"]
keywords: ["routeros", "example", "library", "access", "minimal", "identity", "how", "api"]
source: https://techoverflow.net/2023/09/16/how-to-access-routeros-api-in-python-using-the-routeros-library-minimal-example/
source_anchor: ""
source_lines: [1, 12]
section: "Gemini_Google"
---
This example uses the routeros library from PyPI (GitHub) to access the MikroTik API and extract the system identity.

routeros_minimal_example.py

```
#!/usr/bin/env python3
from routeros import login
routeros = login('admin', 'abc123abc', '192.168.88.1')
output = routeros('/system/identity/print')
# Extract the one identity string from the list of dictionaries
print(output[0]['name'])
```
