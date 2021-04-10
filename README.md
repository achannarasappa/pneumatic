# pneumatic

## Usage

```sh
go tool go2go translate ./**/*.go2 && go tool go2go run main.go2
```

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

```sh
find . -maxdepth 1 -type d \( ! -name .\* \) -exec bash -c "cd '{}' && go tool go2go test" \;
```

## Status

`pneumatic` is for experimental purposes only since the `go2go` toolchain is experimental itself and many features found in the latest stable version of go are not implemented or do not work as expected.

### Known Issues

* Standard recursive test command (`go test ./...`) does not work with go2go

### Future

`pneumatic` will be updated to use the expected beta release of go 1.18 when available sometime in 2021.

The idea is that this library can be evolved as the generics implementation is developed in go 1.18.

## Background

`pneumatic` uses an experimental compiler `go2go` which implements the features and contract defined in the [proposal for go generics](https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md). Generics are expected to  released as part of go 1.18 in early 2022. `go2go` may fall behind changes in the proposal but the expectation is that major aspects of the proposal will be retained in the final implementation.