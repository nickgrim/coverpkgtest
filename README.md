# `-coverpkg` does not work how I think

This repo has two packages, `foo` and `bar`. `foo` has 100% test coverage, `bar` has 0%.

```
$ go test -count=1 ./...
?       github.com/nickgrim/coverpkgtest/bar    [no test files]
ok      github.com/nickgrim/coverpkgtest/foo    0.001s

$ go test -count=1 ./... -coverprofile=coverage.out
?       github.com/nickgrim/coverpkgtest/bar    [no test files]
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 100.0% of statements

$ go test -count=1 ./... -coverprofile=coverage.out -coverpkg=./...
?       github.com/nickgrim/coverpkgtest/bar    [no test files]
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 50.0% of statements in ./...

$ go test -count=1 ./... -coverprofile=coverage.out -coverpkg=github.com/nickgrim/coverpkgtest/foo
?       github.com/nickgrim/coverpkgtest/bar    [no test files]
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 100.0% of statements in github.com/nickgrim/coverpkgtest/foo

$ go test -count=1 ./... -coverprofile=coverage.out -coverpkg=github.com/nickgrim/coverpkgtest/bar
?       github.com/nickgrim/coverpkgtest/bar    [no test files]
ok      github.com/nickgrim/coverpkgtest/foo    0.001s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar

$ go test -count=1 ./... -coverprofile=coverage.out -coverpkg=github.com/nickgrim/coverpkgtest/{foo,bar}
?       github.com/nickgrim/coverpkgtest/bar    [no test files]
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar
```
