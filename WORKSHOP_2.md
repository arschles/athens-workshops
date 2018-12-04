# Workshop 2: Let's Work With Athens!

We're going to build [hugo](https://github.com/gohugo/hugo) _again_ in this workshop! If you forgot what Hugo is, check out the intro in [the first workshop](./WORKSHOP_1.md).

Last time, we built Hugo with Go Modules, but we downloaded modules directly from version control systems like GitHub or GitLab. This time, we're still going to use modules, but we'll make one tiny change and download them from a module API instead.

>If you haven't done [workshop 1](./WORKSHOP_1.md) already, can you go finish that one up first? You'll be all set up with everything you need to know for this one.

**This workshop will take about 20 minutes**

Let's get started :white_check_mark:

1. We should already have the [Hugo source code](https://github.com/gohugoui/hugo) from the [first workshop](./WORKSHOP_1.md)
1. Let's remove the `hugo` binary we built last time (so we can show that it built again)
    1. On Mac/Linux, `rm hugo`
1. Now let's tell the `go` CLI to download dependencies from Athens. Set the `GOPROXY` environment variable to `https://athens.azurefd.net`
    1. This is an experimental Athens proxy on the internet that serves modules over a CDN
    1. On Mac/Linux, run `export GOPROXY=https://athens.azurefd.net`
    2. On Windows PowerShell, run `$env:GOPROXY = "https://athens.azurefd.net"`
1. Now the CLI will use the modules HTTP API to download _all_ the modules we need
1. Next, let's clear out the global cache on our machines to make sure we're downloading everything from the Athens server
    1. `sudo rm -rf $GOPATH/pkg/mod` on Mac/Linux
    2. `Remove-Item -Recurse -Force %GOPATH%/pkg/mod` on Windows Powershell (see [here](https://stackoverflow.com/questions/1752677/how-to-recursively-delete-an-entire-directory-with-powershell-2-0)) for info
1. Now, let's try a `go build` again.
    1. Notice that we have the same log output?
    1. ... this time it's downloading over the HTTP API.
1. After it's built, let's run `./hugo` and see it already got built
1. Just like we did before, let's check out our local cache to see that it was filled up again
    1. On Mac/Linux, it's at `$GOPATH/pkg/mod`
    1. On Windows, it's at `%GOPATH%/pkg/mod`

Now let's do a final bonus round!

1. I'm going to start up an Athens server on my local machine
    - This is the command I'm gonna run:
    
    ```
    docker run --rm \
    -p 3000:3000 \
    -v $PWD/athens_storage:/athens \
    -e ATHENS_STORAGE_TYPE=disk \
    -e ATHENS_DISK_STORAGE_ROOT=/athens \
    gomods/athens:v0.2.0
    ```
1. Next, I'm going to fire up an [ngrok](https://ngrok.com) tunnel so that you can access it over the internet
1. Now, point your `GOPROXY` to that address and clear your cache once more
1. When you `go build`, let's look at the log messages that my local Athens spits out
1. ... and that the Athens database on my local machine is filling up!

**We've made Athens our cache in the cloud! :cloud:**
