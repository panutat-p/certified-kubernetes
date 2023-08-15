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
bind ^a redo main
bind ^x cut main
bind ^c copy main
bind ^v paste main
bind ^f whereis main
bind ^g wherewas main

syntax yaml "\.ya?ml$"
color magenta "^\s*[A-Za-z0-9_-]+:"
color brightred ":"
```

`.bashrc`
```
export KUBE_EDITOR=nano
export EDITOR=nano
```

# Utilities

```shell
alias c='clear'
alias l='ls -laF'
alias ll='ls -lF'
```

```shell
enc() {
  echo -n "$1" | base64
  echo
}

dec() {
  echo -n "$1" | base64 -d
  echo
}
```
