# Nano

```shell
whereis nano
```

```shell
nano --version
```

```shell
man nanorc
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

bind ^z undo main
bind ^x redo main
bind ^c copy main
bind ^v paste main
bind ^f whereis main
bind ^g wherewas main

syntax yaml "\.ya?ml$"
color magenta "^\s*[A-Za-z0-9_-]+:"
color brightred ":"
```
