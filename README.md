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
ok      github.com/nickgrim/coverpkgtest/bar    0.003s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/foo    0.001s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 100.0% of statements in github.com/nickgrim/coverpkgtest/bar
+ go test -count=1 ./... -cover -coverpkg=github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/foo    0.002s  coverage: 100.0% of statements in github.com/nickgrim/coverpkgtest/foo
ok      github.com/nickgrim/coverpkgtest/intn   0.003s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo
+ go test -count=1 ./... -cover -coverpkg=github.com/nickgrim/coverpkgtest/foo,github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/bar    0.002s  coverage: 0.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/foo    0.005s  coverage: 50.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 50.0% of statements in github.com/nickgrim/coverpkgtest/foo, github.com/nickgrim/coverpkgtest/bar
+ go test -count=1 ./... -cover -coverpkg=all
ok      github.com/nickgrim/coverpkgtest/bar    0.003s  coverage: 14.1% of statements in all
ok      github.com/nickgrim/coverpkgtest/foo    0.003s  coverage: 14.1% of statements in all
ok      github.com/nickgrim/coverpkgtest/intn   0.003s  coverage: 14.5% of statements in all
+ go test -count=1 ./... -cover -coverpkg=./...
ok      github.com/nickgrim/coverpkgtest/bar    0.001s  coverage: 0.0% of statements in ./...
ok      github.com/nickgrim/coverpkgtest/foo    0.001s  coverage: 50.0% of statements in ./...
ok      github.com/nickgrim/coverpkgtest/intn   0.002s  coverage: 50.0% of statements in ./...
```

The coverage percentages for `-coverpkg=all` seem to vary from run-to-run as well, which Ain’t Great.

---

Removing (the basically-empty) `intn/intn.go` gets you the build failures discussed [here](https://github.com/golang/go/issues/27333):
```
$ ./run-tests
…
+ go test -count=1 ./... -cover -coverpkg=all
go build github.com/nickgrim/coverpkgtest/intn: no non-test Go files in /home/nick/src/coverpkgtest/intn
FAIL    github.com/nickgrim/coverpkgtest/bar [build failed]
FAIL    github.com/nickgrim/coverpkgtest/foo [build failed]
ok      github.com/nickgrim/coverpkgtest/intn   0.003s  coverage: 14.1% of statements in all
FAIL
+ go test -count=1 ./... -cover -coverpkg=./...
go build github.com/nickgrim/coverpkgtest/intn: no non-test Go files in /home/nick/src/coverpkgtest/intn
FAIL    github.com/nickgrim/coverpkgtest/bar [build failed]
FAIL    github.com/nickgrim/coverpkgtest/foo [build failed]
ok      github.com/nickgrim/coverpkgtest/intn   0.001s  coverage: 50.0% of statements in ./...
FAIL
```
