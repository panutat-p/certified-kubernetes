# Nano

## Version

```shell
nano --version
```

## Configurations

`~/.nanorc`
```
set linenumbers
set tabstospaces
set tabsize 2
set autoindent
set indicator
```

## Syntax Highlighting

https://github.com/serialhex/nano-highlight/tree/master

```shell
git clone https://github.com/serialhex/nano-highlight ~/.nano
```

`~/.nanorc`
```
include "~/.nano/nanorc.nanorc"
include "~/.nano/yaml.nanorc"
```
