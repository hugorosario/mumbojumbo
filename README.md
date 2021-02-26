# Mumbo jumbo

A small tool to obfuscate strings in your Go code.

This is a fork from [mumbojumbo](https://github.com/jeromer/mumbojumbo) to add some more functionality and customization of the function names.

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

[mumbojumbo](https://github.com/hugorosario/mumbojumbo) tries to mitigate this
problem by obfuscating the string so it is no longer searchable with
[strings](https://linux.die.net/man/1/strings).


## Getting Started

You can install mumbojumbo by running:

    go get github.com/hugorosario/mumbojumbo/...

## Usage

Once [mumbojumbo](https://github.com/hugorosario/mumbojumbo) is installed just run:

    mumbojumbo -s=foo -p=bar -op=shr -f=getMyString

where `foo` is the string you want to obfuscate and `bar` is the name of the go
package which will be generated, `shr` is the operation function name which will be used in the obsfuscation algorithm and `getMyString` is the name of the getter function you will use to retrieve the real value of your string.

For example, imagine I want to obfuscate the string `some secret` in pkg
`main`:

    mumbojumbo -s="some secret" -p=main -op=mov -f=GetSecret > secret.go

The following code will be generated a `secret.go` file with the content:

```go
// CODE GENERATED BY mumbojumbo 1.2 (https://github.com/hugorosario/mumbojumbo) DO NOT EDIT !!!!

package main

import (
	"unsafe"
)

func mov() uint8 {
	return uint8(unsafe.Sizeof(true))
}

//GetSecret getter function
func GetSecret() string {
	return string(
		[]byte{
			((((mov()<<mov()^mov())<<mov()|mov())<<mov()<<mov()<<mov()|mov())<<mov() | mov()),
			(((((mov()<<mov()^mov())<<mov()<<mov()|mov())<<mov()|mov())<<mov()|mov())<<mov() | mov()),
			((((mov()<<mov()^mov())<<mov()<<mov()|mov())<<mov()|mov())<<mov()<<mov() | mov()),
			(((mov()<<mov()^mov())<<mov()<<mov()<<mov()|mov())<<mov()<<mov() | mov()),
			mov() << mov() << mov() << mov() << mov() << mov(),
			((((mov()<<mov()^mov())<<mov()|mov())<<mov()<<mov()<<mov()|mov())<<mov() | mov()),
			(((mov()<<mov()^mov())<<mov()<<mov()<<mov()|mov())<<mov()<<mov() | mov()),
			(((mov()<<mov()^mov())<<mov()<<mov()<<mov()<<mov()|mov())<<mov() | mov()),
			(((mov()<<mov()^mov())<<mov()|mov())<<mov()<<mov()<<mov() | mov()) << mov(),
			(((mov()<<mov()^mov())<<mov()<<mov()<<mov()|mov())<<mov()<<mov() | mov()),
			(((mov()<<mov()^mov())<<mov()|mov())<<mov()<<mov() | mov()) << mov() << mov(),
		},
	)
}

```

Now import `secret.go` in your project and call `fmt.Println(GetSecret())` you should
see "some secret"

Run `mumbojumbo --help` to get help

## License

This project is licensed under the BSD 3-Clause License - see the [LICENSE](LICENSE) file for details

## Acknowledgments

* Thanks to jeromer for the original [mumbojumbo](https://github.com/jeromer/mumbojumbo) which this was based on.
* Thanks [GH0st3rs](https://github.com/GH0st3rs) for the [obfuscation code](https://github.com/GH0st3rs/obfus/blob/master/obfus.go)
