# `go test -coverpkg` does not work how I think

The code in `foo` is 100% covered by the test in `foo/foo_test.go`. The code in `bar` is 100% covered by the test in `intn/integration_test.go`.

I can’t seem to find a way to run _all_ the tests, and get coverage results saying that both `foo` and `bar` have 100% coverage.

```
$ ./run-tests
+ go test -count=1 ./... -cover
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 100.0% of statements
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: [no statements]
+ go test -count=1 ./... -cover -coverpkg=github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/bar    0.001s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/foo    0.001s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 100.0% of statements in github.com/nickgrim/coverpkgtest/bar
+ go test -count=1 ./... -cover -coverpkg=github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/bar    0.001s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 100.0% of statements in github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo
+ go test -count=1 ./... -cover -coverpkg=github.com/nickgrim/coverpkgtest/foo,github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/foo    0.006s  coverage: 50.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 50.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
+ go test -count=1 ./... -cover -coverpkg=all
ok      github.com/nickgrim/coverpkgtest/bar    0.003s  coverage: 14.1% of statements in all
ok      github.com/nickgrim/coverpkgtest/foo    0.003s  coverage: 14.3% of statements in all
ok      github.com/nickgrim/coverpkgtest/intn   0.003s  coverage: 14.4% of statements in all
+ go test -count=1 ./... -cover -coverpkg=./...
ok      github.com/nickgrim/coverpkgtest/bar    0.003s  coverage: 0.0% of statements in ./...
ok      github.com/nickgrim/coverpkgtest/foo    0.003s  coverage: 50.0% of statements in ./...
ok      github.com/nickgrim/coverpkgtest/intn   0.004s  coverage: 50.0% of statements in ./...
```
