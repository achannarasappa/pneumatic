# pneumatic

Pneumatic is a type-safe functional library for Go that uses Go 1.18 generics

## Usage

```sh
go get github.com/achannarasappa/pneumatic
```

TBD example

## Development

### Setup

1. Clone go2go:
```sh
git clone git@github.com:golang/go.git $HOME/go2
git checkout dev.go2go
```

2. Update `go` command to point to go2go
```sh
export GOROOT="$HOME/go2"
export PATH="$PATH:$GOROOT/bin"

alias go='$HOME/go2/bin/go'
```

3. Clone pneumatic
```sh
git clone git@github.com:achannarasappa/pneumatic.git
```

4. Symlink the working repo into the GOPATH
```sh
mkdir -p $GOROOT/src/github.com/achannarasappa
ln -s ./pneumatic $GOROOT/src/github.com/achannarasappa
```

go2go [setup instructions](https://go.googlesource.com/go/+/refs/heads/dev.go2go/README.go2go.md)

### Testing

Example script that demonstrates some of what is possible with `pneumatic`:
```sh
go tool go2go translate ./**/*.go2 && go tool go2go run main.go2
```

Run automated tests:
```sh
find . -maxdepth 1 -type d \( ! -name .\* \) -exec bash -c "cd '{}' && go tool go2go test" \;
```

## Development Status

**Experimental** - this library is for experimental purposes only until a beta Go 1.18 compiler is released

The vision is to evolve `pneumatic` to match the latest implementation of Go generics until the final release in Go 1.18. The API is expected to undergo changes until the v1.0.0 release which will coincide with release of Go 1.18.

### Known Tooling Issues

Since the `go2go` toolchain is experimental itself and many features found in the latest stable version of go are not implemented or do not work as expected.

* No interoperability with Go 1.17 or lower
* Standard recursive test command (`go test ./...`) does not work with go2go
* Third-party libraries do not work out of the box or at all in some cases

## Background

### Motivation

There are many utility and functional programming libraries already for Go but there's a few areas where pneumatic seeks to make improvements:

* Type safety - when building utility functions in Go <=1.17, developers have the options of making their implementation use case specific or using [type assertions](https://golang.org/ref/spec#Type_assertions) or [reflection](https://golang.org/pkg/reflect/) which can lead to panics at runtime and other unexpected behavior
  * Existing utility libraries typically rely on type assertions which pose a serious risk to any workload that can not tolerate a runtime panic (e.g. production applications)
* Functional-first - `pneumatic`'s data processing functions all have an arity of two and receive data as the last argument making it simpler to compose together small functions together into larger ones and promote reusability

### Goals

At the current **experimental** phase of the project, the goals are: 

* Validate hypothesis that there is community interest in a Go functional programming library
* Refine API with community feedback
* Build a feature set that covers most common use cases and enables development of more specialized use cases

### Compiler

`pneumatic` uses an experimental compiler `go2go` which implements the features and contract defined in the [proposal for go generics](https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md). Generics are expected to  released as part of go 1.18 in early 2022. `go2go` may fall behind changes in the proposal but the expectation is that major aspects of the proposal will be retained in the final implementation.