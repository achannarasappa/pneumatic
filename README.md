# pneumatic

Pneumatic is a functional library for Go that uses Go 1.18 generics


* Utility libraries libraries in Go rely on [type assertions](https://golang.org/ref/spec#Type_assertions) or [reflection](https://golang.org/pkg/reflect/) which can lead to panics at runtime and other unexpected behavior

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

`pneumatic` is for **experimental purposes only** for now

The vision is to evolve `pneumatic` to match the latest implementation of Go generics until the final release in Go 1.18. The API is expected to undergo changes until the v1.0.0 release which will coincide with release of Go 1.18.

### Known Tooling Issues

Since the `go2go` toolchain is experimental itself and many features found in the latest stable version of go are not implemented or do not work as expected.

* No interoperability with Go 1.17 or lower
* Standard recursive test command (`go test ./...`) does not work with go2go

## Background

`pneumatic` uses an experimental compiler `go2go` which implements the features and contract defined in the [proposal for go generics](https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md). Generics are expected to  released as part of go 1.18 in early 2022. `go2go` may fall behind changes in the proposal but the expectation is that major aspects of the proposal will be retained in the final implementation.