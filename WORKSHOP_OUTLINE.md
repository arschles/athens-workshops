# Workshop Outline

Total length: 90 mins

# Intro to Go (10 minutes)

History

- came from Google
- intended originally for busy distributed systems
- written with mono-repos in mind
- open source community adapted it to tons of uses (web develoment, robotics, AI, ...)

# History of Dependency Management (10 mins)

- not much attention initially (see monorepos above)
- go get came out - shared GOPATH, no versioning per package
- Godep, gb, other systems set GOPATH to current dir to isolate GOPATH per project
- Each project (i.e. current dir) can version its dependencies independently
- vendor directory - official way to isolate dependencies per project (started with the "vendor experiment")
- glide & dep - dependency management systems like Godep that use vendor dir
    - dep was an official experiment, collab between Go team and community
    - init by crawling imports in the project
    - ...calculate "transitive dependencies"
    - ...store everything in a lock file
- then modules come out

# Modules? (30 total mins)

## History (10 mins)

- Module = Go code (formerly called a package) + version
- New, official implementation of modern dependency management for Go
- Not very similar to dep
- Still isolates the GOPATH to a single project
- Doesn't even require you to put your project inside the GOPATH (YAYYY!)
- All built into the go CLI
- Still crawls the import path, stores all dependencies in a file, and has a lock file
- Has a global cache on-disk, in the GOPATH
- The cache is for modules, which have versions
- By default, still downloads from the VCS (Github, etc...)
- BUT, it supports an HTTP API!
- ... so we can build servers that abstract away all the VCS stuff
- ... we've been calling these servers module proxies

## Workshop (20 mins)

Let's do stuff!

- Download Hugo
- See the `go.mod` and `go.sum`. The former is the "top level" and the latter is everything
- `go build`
- Watch it download stuff!
- The program is built
- Check out what's in the global on-disk cache
- Notice it looks like the GOPATH, but:
    - It has versions in it
    - Your project is completely independent of it

# Athens! (40 mins)

## Overview (10 mins)

- It's a module proxy :)
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
