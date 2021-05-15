# pneumatic

Pneumatic is a practical type-safe functional library for Go that uses Go 1.18 generics

```go
var getTopUser = Compose3[[]Post, []Post, Post, UserLevelPoints](
	// Sort users by points
	SortBy(func (prevPost Post, nextPost Post) bool {
		return prevPost.User.Points > nextPost.User.Points
	}),
	// Get top user by points
	Head[Post],
	// Summarize user from post
	func(post Post) UserLevelPoints {
		return UserLevelPoints{
			FirstName:   post.User.Name.First,
			LastName:    post.User.Name.Last,
			Level:       post.Level,
			Points:      post.User.Points,
			FriendCount: len(post.User.Friends),
		}
	},
)

var getTopUsers = Compose3[[]Post, map[string][]Post, [][]Post, []UserLevelPoints](
	// Group posts by level
	GroupBy(func (v Post) string { return v.Level }),
	// Covert map to values only
	Values[[]Post, string],
	// Iterate over each nested group of posts
	Map(getTopUser),
)

posts, _ := getPosts("data.json")
topUsers := getTopUsers(posts)

fmt.Printf("%+v\n", topUsers)
// [{FirstName:Ferguson LastName:Bryant Level:gold Points:9294 FriendCount:3} {FirstName:Ava LastName:Becker Level:silver Points:9797 FriendCount:2} {FirstName:Hahn LastName:Olsen Level:bronze Points:9534 FriendCount:2}]
```
[Example here](https://github.com/achannarasappa/pneumatic/blob/main/examples/functional-pipeline/main.go2)

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

### Usage

Example script that demonstrates some of what is possible with `pneumatic`:
```sh
go tool go2go translate ./**/*.go2 && go tool go2go run examples/functional-pipeline/main.go2
```

Note: The path to the data file will need to be replaced in order for the example to work. Issues with go path and go2go require this manual step

### Tests
Run automated tests:
```sh
find . -maxdepth 1 -type d \( ! -name .\* \) -exec bash -c "cd '{}' && go tool go2go test" \;
```

## API Reference

Godoc is one of the unsupported features of go2go so there is no API reference available on pkg.go.dev but you can find each function's documentation below:

* [Slice](https://github.com/achannarasappa/pneumatic/blob/main/slice/slice.go2)
* [Map](https://github.com/achannarasappa/pneumatic/blob/main/maps/maps.go2)
* [Function](https://github.com/achannarasappa/pneumatic/blob/main/function/function.go2)

## Development Status & Roadmap

**Experimental** (Pre-alpha) - for experimental purposes only until a beta Go 1.18 compiler is released

The vision is to evolve `pneumatic` to match the latest implementation of Go generics until the final release in Go 1.18. The API is expected to undergo changes until the v1.0.0 release which will coincide with release of Go 1.18.

* Experimental (Pre-alpha, Alpha releases)
  * Internal Tasks
    * Design - refine and expand API with community feedback
    * Implementation - build a feature set that covers most common use cases and enables development of more specialized use cases
    * Documentation - cover all implemented functions with basic godoc-compatible documentation, create examples for common use cases that demonstrate end user value
    * Community - validate hypothesis that there is community interest in a Go functional programming library, draft contributing guidelines, establish feature proposal, design, review, acceptance process and criteria
    * Testing - cover major code pathways and all functions with at least one test
  * External Milestones
    * Release a dev build of Go 1.18 that includes generics
* Refinement (Beta, release candidate releases)
  * Internal Tasks
    * Design - continue to gather community feedback, finalize API and feature set for GA release
    * Documentation - publish to go.pkg.dev and update documentation for clarity and formatting
    * Automation & tooling - improve automation of dev lifecycle
    * Testing - increase code coverage to 100%, implement more ergonomic testing tooling
    * Performance - baseline performance benchmarks, identify opportunities to improve performance
  * External Milestones
    * Stable release of generics in Go 1.18
* Stable (General Availability release)
  * Internal Milestones - TBD

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
* Functional-first - `pneumatic`'s functions all receive data as the last argument making it simpler to compose together small functions together into larger ones and promote reusability

### Compiler

`pneumatic` uses an experimental compiler `go2go` which implements the features and contract defined in the [proposal for go generics](https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md). Generics are expected to  released as part of go 1.18 in early 2022. `go2go` may fall behind changes in the proposal but the expectation is that major aspects of the proposal will be retained in the final implementation.