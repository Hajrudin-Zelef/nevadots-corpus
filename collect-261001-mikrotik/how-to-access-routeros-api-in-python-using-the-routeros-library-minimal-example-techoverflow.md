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