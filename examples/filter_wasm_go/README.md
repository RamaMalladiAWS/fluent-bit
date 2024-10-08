# Fluent Bit / filter_wasm_go

This source source tree provides an example of WASM filter program with WASI mode.

## Prerequisites

Tested on

* TinyGo
  * [tinygo](https://tinygo.org/) tinygo version 0.32.0 linux/amd64 (using go version go1.22.5 and LLVM version 18.1.2)

For Ubuntu, it's easy to install with:

```console
$ wget https://github.com/tinygo-org/tinygo/releases/download/v0.32.0/tinygo_0.32.0_amd64.deb
$ sudo dpkg -i tinygo_0.32.0_amd64.deb
```

## How to build

Execute _tinygo build_ as follows:

```console
$ tinygo build -target=wasi -o filter.wasm filter.go
```

Finally, under the same directory, `*.wasm` file will be created:

```console
$ ls *.wasm
filter.wasm
```

## How to confirm WASM filter integration

Create fluent-bit configuration file as follows:

```ini
[SERVICE]
    Flush        1
    Daemon       Off
    Log_Level    info
    HTTP_Server  Off
    HTTP_Listen  0.0.0.0
    HTTP_Port    2020

[INPUT]
    Name dummy
    Tag dummy.local

[FILTER]
    Name wasm
    match dummy.*
    Event_Format json
    WASM_Path /path/to/filter.wasm
    Function_Name go_filter
    accessible_paths .,/path/to/fluent-bit

[OUTPUT]
    Name  stdout
    Match *
```
