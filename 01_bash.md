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

## Nano configurations

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

bind ^a redo main
bind ^z undo main
bind ^x cut main
bind ^c copy main
bind ^v paste main
bind ^f whereis main
bind ^g wherewas main

syntax yaml "\.ya?ml$"
color magenta "^\s*[A-Za-z0-9_-]+:"
color brightred ":"
```

## Bash configurations

`~/.bashrc`
```bash
export EDITOR=nano
export KUBE_EDITOR=nano

KUBECONFIG=~/.kube/config

alias c='clear'
alias l='ls -laF'
alias ll='ls -lF'

enc() {
  echo -n "$1" | base64
  echo
}

dec() {
  echo -n "$1" | base64 -d
  echo
}

kn() {
  [ "$1" ] && kubectl config set-context --current --namespace $1 || kubectl config view --minify | grep namespace
}

kx() {
  [ "$1" ] && kubectl config use-context $1 || kubectl config current-context
}
```
