---
id: collect-261121-veille-ia/fortinet/github-fortinet-solutions-cse-fortiosapi-examples-separate-and-use-git-submodules-to-clean-the-forti
title: "GitHub - fortinet-solutions-cse/fortiosapi-examples: Separate and use git submodules to clean the fortiosapi repo"
domain: fortinet
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["fortiosapi", "submodules", "github", "fortinet-solutions-cse", "fortiosapi-examples", "separate", "use", "git"]
source: https://github.com/fortinet-solutions-cse/fortiosapi-examples
source_anchor: ""
source_lines: [1, 14]
section: "Copilot_Microsoft"
---
To use the library in the submodule instead of the pypi one set variable:

`PYTHONPATH='./fortiosapi'`
in python console

`os.environ['PYTHONPATH'] = './fortiosapi'`
To correctly get the referenced submodules please use the --recursive option of git clone like this:

`git clone https://github.com/fortinet-solutions-cse/fortiosapi-examples.git --recursive`
pull

```
git pull --recurse-submodule
```
