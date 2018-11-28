# Workshop 1: Let's Work With Modules!

We're going to build [hugo](https://github.com/gohugo/hugo) in this workshop. Hugo is a super fast static site generator written in Go. It's widely used on the internet, including to build the [Athens documentation site](https://docs.gomods.io), [my personal site](https://arschles.com), and the [Go in 5 Minutes site](https://goin5minutes.com) (all shameless plugs, of course :grinning:)

**This workshop will take about 20 minutes**

Let's get started :white_check_mark:

1. Download [Hugo](https://github.com/gohugoio/hugo) with `git clone https://github.com/gohugoio/hugo.git`
2. Check out the `go.mod` and `go.sum` files
    1. `go.mod` is the "top level" dependencies. These are the ones that you import directly from your code. You can change the versions in here or add/remove lines if you want
    2. `go.sum` is every single depencency your code needs to compile. It's the modules in `go.mod` plus all the modules that those `import`, and the modules that those `import`, and so on... These are called _transitive dependencies_
    3. `go.sum` also has _hashes_ of module contents. Those are important so the `go` tool can quickly check if a module's code is the same as it expects (i.e. if it's downloading the code onto your machine for the first time)
3. Run `go build`
    1. Those are new log lines!
4. Check out `./hugo` - it builds (and runs) now!!!
5. Remember that global cache? Check out `$GOPATH/pkg/mod` (Mac/Linux) / `%GOPATH%/pkg/mod` (Windows)
    1. If you haven't set your `GOPATH`, see https://rakyll.org/default-gopath/ for how to find it
6. That looks kinda like the old `GOPATH/src`  Notice it looks like the GOPATH, but:
    - It has versions in it
    - Your project is independent of it: you can delete the cache (`sudo rm -rf $GOPATH/pkg/mod` on Mac/Linux), and/or you can move your code if you want, and things still work.
