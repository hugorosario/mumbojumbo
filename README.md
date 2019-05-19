# Mumbo jumbo

A small tool to obfuscate strings in your Go code

## Description

When you want to store specific strings in your go code they are automatically
searchable in the go binary.

For example take the following code (`x.go`):

```go
    package main

    import (
        "fmt"
    )

    const (
        C = "hello"
    )

    func main() {
        fmt.Println(C)
    }
```

Now run `go build ./x.go` to build it and the run `strings ./x | grep hello`.

Surprise ! `hello` is visible to anyone who has access the go binary.

[mumbojumbo](https://github.com/jeromer/mumbojumbo) tries to mitigate this
problem by obfuscating the string so it is no longer searchable with
[strings](https://linux.die.net/man/1/strings).


## Getting Started

You can install mumbojumbo by running:

    go get github.com/jeromer/mumbojumbo/...

## Usage

Once [mumbojumbo](https://github.com/jeromer/mumbojumbo) is installed just run:

    mumbojumbo -s=foo -p=bar

where `foo` is the string you want to obfuscate and `bar` is the name of the go
package which will be generated.

For example, imagine I want to obfuscate the string `some secret` in pkg
`foo`:

    mumbojumbo -s="some secret" -p=foo | goimports > foo.go

The following code will be generated:

```go
// CODE GENERATED BY mumbojumbo 1.0 (https://github.com/jeromer/mumbojumbo) DO NOT EDIT !!!!

package foo

import (
	"unsafe"
)

const (
	EAX = uint8(unsafe.Sizeof(true))
)

func Get() string {
	return string(
		[]byte{
			((((EAX<<EAX|EAX)<<EAX|EAX)<<EAX<<EAX<<EAX|EAX)<<EAX ^ EAX),
			(((((EAX<<EAX|EAX)<<EAX<<EAX|EAX)<<EAX|EAX)<<EAX^EAX)<<EAX | EAX),
			((((EAX<<EAX|EAX)<<EAX<<EAX|EAX)<<EAX|EAX)<<EAX<<EAX ^ EAX),
			(((EAX<<EAX|EAX)<<EAX<<EAX<<EAX|EAX)<<EAX<<EAX | EAX),
			EAX << EAX << EAX << EAX << EAX << EAX,
			((((EAX<<EAX|EAX)<<EAX|EAX)<<EAX<<EAX<<EAX|EAX)<<EAX ^ EAX),
			(((EAX<<EAX|EAX)<<EAX<<EAX<<EAX|EAX)<<EAX<<EAX | EAX),
			(((EAX<<EAX|EAX)<<EAX<<EAX<<EAX<<EAX|EAX)<<EAX | EAX),
			(((EAX<<EAX|EAX)<<EAX|EAX)<<EAX<<EAX<<EAX | EAX) << EAX,
			(((EAX<<EAX|EAX)<<EAX<<EAX<<EAX|EAX)<<EAX<<EAX | EAX),
			(((EAX<<EAX|EAX)<<EAX|EAX)<<EAX<<EAX | EAX) << EAX << EAX,
		},
	)
}

```

Now import `foo.go` in your project and call `fmt.Println(foo.Get())` you should
see "some secret"

Run `mumbojumbo --help` to get help

## License

This project is licensed under the BSD 3-Clause License - see the [LICENSE](LICENSE) file for details

## Acknowledgments

* Thanks [GH0st3rs](https://github.com/GH0st3rs) for the [obfuscation code](https://github.com/GH0st3rs/obfus/blob/master/obfus.go)
