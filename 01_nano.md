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
set zap
set tabsize 2
set tabstospaces
set autoindent

bind ^X cut main
bind ^C copy main
bind ^V paste main
bind ^F whereis main
bind ^G wherewas main
bind ^Q findprevious main
bind ^W findnext main

syntax "yaml" "\.ya?ml$"
color magenta "^\s*[A-Za-z0-9_-]+:"
color brightred ":"
```
