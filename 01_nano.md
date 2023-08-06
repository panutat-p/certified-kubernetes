# Nano

## Version

```shell
nano --version
```

## Configurations

https://bash-prompt.net/guides/nanorc-settings

https://github.com/serialhex/nano-highlight/blob/master/yaml.nanorc

```shell
ls -la /usr/share/nano
```

`~/.nanorc`
```
set linenumbers
set tabstospaces
set tabsize 2
set autoindent

syntax "yaml" "\.ya?ml$"
color magenta "^\s*[A-Za-z0-9_-]+:"
color brightred ":"
```
