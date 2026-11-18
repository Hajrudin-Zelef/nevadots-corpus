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