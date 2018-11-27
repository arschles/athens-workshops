# Workshop Outline

Total length: 90 mins

# Intro to Go (10 minutes)

History

- Came from Google
- Intended originally for busy distributed systems
- Written with mono-repos in mind
    - One repository, many projects, one shared dependency tree
- Open source community adapted it to tons of uses (web develoment, robotics, AI, ...)

# History of Dependency Management (10 mins)

- Not much attention initially (see monorepos above)
- `go get` came out - shared `GOPATH`, no versioning per package, non-deterministic builds over time
- [godep](https://github.com/tools/godep), [gb](https://github.com/constabulary/gb), other systems set `GOPATH` to current dir to isolate it per project
    - So that each project can version its dependencies independently
- `vendor` directory - official way to isolate dependencies per project (started with the "vendor experiment")
- [glide](https://github.com/Masterminds/glide) & [dep](https://github.com/golang/dep) - dependency management systems like godep that use the vendor directory natively
- `dep` was an official experiment for package management, one of the first "official" collaborations between the Go team and community
    - It could initialize your current project by crawling your imports
    - ... it then calculates "transitive dependencies" (imports-of-imports, imports-of-imports-of-imports, etc...)
    - ... and stores the entire list of all your dependencies in a lock file

And then, [Go Modules](https://github.com/golang/go/wiki/Modules) are released.

# Modules? (30 total mins)

## History (10 mins)

- Module = Go code (formerly called a package), and a version
- New, official implementation of modern dependency management for Go
- Pretty different from dep because its core dependency algorithm is different
- Still isolates the GOPATH to a single project
- ... in fact you can put your project outside the `GOPATH` now :tada: :rocket:!
- It's built into all the familiar commands - `go get`, `go build`, `go install`, etc...
- You can still initialize your project from Glide and Dep files, or by crawling the import path from scratch
- Has a global cache on-disk, in the `GOPATH`
- It's similar to the vendor directory but the cache is for modules and their versions so you can safely use it for all projects on your computeer
- `go get` and friends still work for the same version control systems like GitHub, GitLab, etc...
- ... and for "vanity" imports like gopkg.in

But modules also can fetch code from an HTTP API, so we can build HTTP servers to serve them.

These servers are called _module proxies_, and they abstract away all the VCS stuff.

We'll do a workshop with just modules first, and then we'll start using Athens after that.

## Workshop (20 mins)

Let's work with Modules!

1. Download [Hugo](https://github.com/gohugoio/hugo) with `git clone https://github.com/gohugoio/hugo.git`
1. Check out the `go.mod` and `go.sum` files
    1. `go.mod` is the "top level" dependencies. These are the ones that you import directly. You can change the versions in here or add/remove lines if you want
    1. `go.sum` is every single depencency your code needs to compile
1. Run `go build`
    1. Those are new log lines!
1. Check out `./hugo` - it builds (and runs) now!!!
1. Remember that global cache? Check out `$GOPATH/pkg/mod` (Mac/Linux) / `%GOPATH%/pkg/mod` (Windows)
    1. If you haven't set your `GOPATH`, see https://rakyll.org/default-gopath/ for how to find it
1. That looks kinda like the old `GOPATH/src`  Notice it looks like the GOPATH, but:
    - It has versions in it
    - Your project is independent of it: you can delete the cache, and you can move your code

# Athens! (40 mins)

## Overview (10 mins)

- It's one of the first module proxy servers :smile:
- It serves zip files of source code (the API requires this)
- ... so modules are smaller - no more Git trees
- And it supports a CDN, so it's fast

## Workshop (20 mins)

- Let's use Hugo again
- Set `GOPROXY` env var to remote Athens (`athens.azurefd.net`)
- Now we're using the HTTP API for _all_ dependencies we need
- Clear out the global cache
- Try a `go build` again
- It builds again!
- Check out everything in the global on-disk cache. It's the same as before

## Demo (10 mins)

Context:
- What if you want a local proxy?
    - You have internal, private code
    - You can't access the internet
    - WHAT IF YOU'RE ON THE PLANE!!!?!?!?!?!?!?
- Just set your `GOPROXY` to a different place

Demo:

- Run Athens locally (easy to do with Docker)
- Do a `go build`
- Check out local Athens's cache
    - It's different from the Go on-disk cache
- Shut down the WIFI
- Clear global on-disk cache
- Set `GOPROXY` to localhost
- Check it out - we still build. No remote VCS access, not remote Athens access. YEEAA!
