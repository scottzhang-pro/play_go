
# Code

```go

//hello.go
package hello

import (
	quoteV3 "rsc.io/quote/v3"
)

func Hello() string {
	return quoteV3.HelloV3()
}

func Proverb() string {
	return quoteV3.Concurrency()
}

```

```go
//hello_test.go
package hello

import "testing"

func TestHello(t *testing.T) {
	want := "Hello, world."
	if got := Hello(); got != want {
		t.Errorf("Hello() = %q, want %q", got, want)
	}
}

func TestProverb(t *testing.T) {
	want := "Concurrency is not parallelism."
	if got := Proverb(); got != want {
		t.Errorf("Proverb() = %q, want %q", got, want)
	}
}
```

```go
//go.mod
module scottzhang.pro/hello

go 1.17

require rsc.io/quote/v3 v3.1.0

require (
	golang.org/x/text v0.3.7 // indirect
	rsc.io/sampler v1.3.0 // indirect
)

```

# Use Go Modules

```
module：a collection of Go packages
  |
go.mod: 
    - module's module path
    - dependency requirements
```

Running ENV:

```
1. outside $GOPATH/src <--
2. inside any folder     |
   with:                 |
       - go.mod -------->|
```

Creating a new module:

```
1. create you file.  // status: contains a package
2. cmd: go mod init scottzhang.pro/hello  // make the current directory the root of a module
3. cmd: go test  // test the module
```
- Packages in subdirectories have import paths consisting of the module path plus the path to the subdirectory. 
- So you don't need to run `go mod init` in subdirectories.

go.mod content:

```bash
example.com/hello //main module
golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c //an example of a pseudo-version
rsc.io/quote v1.5.2
rsc.io/sampler v1.3.0

// go command maintains a file named go.sum
// containing the expected cryptographic hashes of the content of specific module versions.
// ensure that future downloads of these modules retrieve the same bits as the first download
```

import other module:

```go
import "rsc.io/quote"
// to check  indirect dependencies
// cmd: go list -m all
```
upgrading dependencies:

```
// here is a example to upgrade text and sampler
cmd: go get golang.org/x/text
cmd: go get rsc.io/sampler
```

now depends on both `rsc.io/quote` and `rsc.io/quote/v3`.

why here is V3?, because:

- Each different major version (v1, v2, and so on) of a Go module uses a different module path
- v3 of rsc.io/quote = rsc.io/quote/v3, Know more visit [here](https://research.swtch.com/vgo-import)

> You can have v1, v2, v3 in same module, but can't with v1.1 and v1.2

To konw what new changes in new version, type `go doc rsc.io/quote/v3`.

Remove no need module:

```
go list -m all  # check
go mod tidy  # cleans up these unused dependencies
```

# Conclusion

- `go mod init` creates a new module, initializing the go.mod file that describes it.
- `go build`, go test, and other package-building commands add new dependencies to go.mod as needed.
- `go list -m all` prints the current module’s dependencies.
- `go get` changes the required version of a dependency (or adds a new dependency).
- `go mod tidy` removes unused dependencies.


# Ref

- [How to Use Go Modules](https://www.digitalocean.com/community/tutorials/how-to-use-go-modules)
- [Using Go Modules](https://go.dev/blog/using-go-modules)
