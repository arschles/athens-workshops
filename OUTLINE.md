# Workshop Outline

Welcome, Gophers! This is the outline for the Athens workshop. If you haven't read the [README](./README.md), can you do me a favor and read that over quickly before you dive in here?

>_Hint: it's ok if you skim that README. I just want you to get a feel for what technologies we're working with today_

We're going to spend about 90 minutes today learning about [Go Modules](https://github.com/golang/go/wiki/Modules) and [Athens](https://docs.gomods.io), and we'll get to do some hands-on coding sessions where we work with these technologies.

Let's get started! :rocket:

# Intro to Go (10 minutes)

Here's a really brief history of the Go programming language. If you're here to learn about modules, you might already use Go, so you can skip this if so.

- Came from Google
- Intended originally for high traffic distributed systems
- Originally built for Google scale software projects
    - Written with mono-repos in mind
    - One repository, many projects, one shared dependency tree
- Open source community adapted it to tons of uses ([web develoment](https://gobuffalo.io), [robotics](https://gobot.io), AI, ...)

# History of Dependency Management (10 mins)

Here's a really brief history of dependency management in Go. If you've used Go for big projects, you might already know some of this, but try to give this a skim even if you do, so you can get a feel for where modules and Athens fit in.

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

All of the technologies above used the `vendor/` directory, and they were really effective. Tons of big projects used them.

But now, [Go Modules](https://github.com/golang/go/wiki/Modules) was released in Go 1.11 and things are changing.

# Modules? (30 total mins)

Let's talk more about Modules!

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
- `go get` and friends still work for the same version control systems (VCS) like GitHub, GitLab, etc...
- ... and for "vanity" imports like gopkg.in

But modules also can fetch code from an HTTP API, so we can build HTTP servers to serve them.

These servers are called _module proxies_, and they abstract away all the VCS stuff.

We'll do a workshop with just modules first, and then we'll start using Athens after that.

## Workshop (20 mins)

See [here](./WORKSHOP_1.md)

# Athens! (40 mins)

Now that we know all about modules, let's learn about how Athens fits in.

## Overview (10 mins)

What happens if you have a "cold" local module cache? Go modules will download all the modules in your `go.sum` for you from the VCS (GitHub, GitLab, etc...). There are two issues with doing that:

1. It can be slow. Behind the scenes, the `go` tool does a full `git clone`, so if the module has a huge git history, that will be really slow!
2. It can fail if the version was deleted, or the entire repository has been moved/deleted :scream:

### Athens to the Rescue! :zap:

Athens is one of the first module proxy servers, and its goal is to solve those above problems. Here's how it works:

1. Downloading modules from the VCS and storing them in its own Database
1. Serving them via the API over a fast CDN
    1. Athens serves a zip file with the module source code at the requested version
    1. ... with no git tree, just the source code, so downloading is much faster than a `git clone` :rocket: :tada:
2. Never changing the modules already in the database, even if they changed in the VCS

## Workshop (20 mins)

See [here](./WORKSHOP_2.md).

## Demo (10 mins)

Now that we finished our last workshop, we've seen how modules work and how Athens can help us with modules. 

Now let's talk about some more uses for Athens:

- What if you have internal/private code?
- What if you want the speed of `localhost`?
- What if you can't access the internet?

Like we saw with the demo, you can set your `GOPROXY` to any proxy address.

I'm going to show a demo where I run an Athens locally, shut down my WiFi, empty my cache, and still be able to build.

1. From the end of the workshop, I have an Athens running
1. ... and its internal database has all the dependencies that Hugo needs to build
1. I'm gonna do some things now:
1. ... clear my local disk cache
1. ... and set my `GOPROXY` to `http://localhost:3000`
1. ... and shut down my WiFi :smiling_imp:
1. ... and finally, do a `go build`

Check it out! We can still build our app without any internet access... :relieved:
