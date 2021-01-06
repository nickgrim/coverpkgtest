# `go test -coverpkg` does not work how I think

The code in `foo` is 100% covered by the test in `foo/foo_test.go`. The code in `bar` is 100% covered by the test in `intn/integration_test.go`.

I can’t seem to find a way to run _all_ the tests, and get coverage results saying that both `foo` and `bar` have 100% coverage.

```
$ ./run-tests
+ go test -count=1 ./... -coverprofile=/dev/null
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements
ok      github.com/nickgrim/coverpkgtest/foo    0.004s  coverage: 100.0% of statements
ok      github.com/nickgrim/coverpkgtest/intn   0.003s  coverage: [no statements]
+ go test -count=1 ./... -coverprofile=/dev/null -coverpkg=github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/foo    0.003s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/intn   0.003s  coverage: 100.0% of statements in github.com/nickgrim/coverpkgtest/bar
+ go test -count=1 ./... -coverprofile=/dev/null -coverpkg=github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 100.0% of statements in github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo
+ go test -count=1 ./... -coverprofile=/dev/null -coverpkg=github.com/nickgrim/coverpkgtest/foo,github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 50.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 50.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
+ go test -count=1 ./... -coverprofile=/dev/null -coverpkg=./...
go build github.com/nickgrim/coverpkgtest/intn: no non-test Go files in /home/nick/src/coverpkgtest/intn
FAIL    github.com/nickgrim/coverpkgtest/bar [build failed]
FAIL    github.com/nickgrim/coverpkgtest/foo [build failed]
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 50.0% of statements in ./...
FAIL
```
