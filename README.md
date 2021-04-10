# Composer

## Setup

```sh
export GOROOT="$HOME/repositories/go2"
export PATH="$PATH:$GOROOT/bin:$GOPATH/bin"

alias go='$HOME/repositories/go2/bin/go'
```

## Usage

```sh
go tool go2go run *.go2
```

## Testing

```sh
find . -maxdepth 1 -type d \( ! -name .\* \) -exec bash -c "cd '{}' && go tool go2go test" \;
```