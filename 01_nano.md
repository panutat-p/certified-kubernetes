# Nano

## Version

```shell
nano --version
```

## Configurations

https://bash-prompt.net/guides/nanorc-settings/

`~/.nanorc`
```
set linenumbers
set tabstospaces
set tabsize 2
set autoindent
set indicator
set mouse

color magenta "^\s*[\$A-Za-z0-9_-]+\:"
```

## Import system nano configurations

> In general, the `yaml.nanorc` file is typically included in the `/usr/share/nano` directory for most Linux distributions,
> including Ubuntu, Debian, and CentOS. This path is a common location for nano's syntax highlighting files.

```shell
ls /usr/share/nano/yaml.nanorc
```

`~/.nanorc`
```
include "/usr/share/nano/yaml.nanorc"
```

## Import custom nano configurations

https://github.com/serialhex/nano-highlight/tree/master

```shell
git clone https://github.com/serialhex/nano-highlight ~/.nano
```

`~/.nanorc`
```
include "~/.nano/nanorc.nanorc"
include "~/.nano/yaml.nanorc"
```
