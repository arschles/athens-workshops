# Workshop 2: Let's Work With Athens!

We're going to build [hugo](https://github.com/gohugoio/hugo) _again_ in this workshop! If you forgot what Hugo is, check out the intro in [the first workshop](./WORKSHOP_1.md).

Last time, we built Hugo with Go Modules, but we downloaded modules directly from version control systems like GitHub or GitLab. This time, we're still going to use modules, but we'll make one tiny change and download them from a module API instead.

>If you haven't done [workshop 1](./WORKSHOP_1.md) already, can you go finish that one up first? In there, you'll learn all about modules, which you'll need to know before you work with Athens in here.

**This workshop will take about 20 minutes**

Let's get started! :white_check_mark: :rocket:

## 1 - Make sure you're using Go version 1.11 or higher

```
go version
```

# 2 - Make sure your `GOPATH` is set

```console
export $GOPATH=$(go env GOPATH)
```

# 3 - Make sure your global on-disk cache is empty

Mac/Linux:
```
sudo rm -rf $GOPATH/pkg/mod
```

Windows Powershell:
```
Remove-Item -Recurse -Force %GOPATH%/pkg/mod
```

See [here](https://stackoverflow.com/questions/1752677/how-to-recursively-delete-an-entire-directory-with-powershell-2-0) for info.

# 4 - Get the [Hugo source code](https://github.com/gohugoui/hugo)

>If you did [workshop 1](./WORKSHOP_1.md), you'll already have the code and should already be inside the checked-out hugo directory, so you don't need to do this step.

```
git clone https://github.com/gohugoio/hugo.git
cd hugo
```

# 5 - Make sure you don't have a `hugo` binary already built

```
rm -f ./hugo
```

# 6 - Tell the `go` CLI to download dependencies from Athens. This is the proxy that runs over a global CDN

>After this step, the `go` tool will only use the HTTP module API to download modules. It won't use `git` or any other VCS

```
export GOPROXY=https://athens.azurefd.net
```

# 7 - Try the `go build` Again

Notice that you see the same log output as in workshop 1. It looks exactly the same, but it's downloading the code over the HTTP API.

# 8 - Run `hugo` Again

```
./hugo version
```

This should output the same thing as in workshop 1

# 9 - Check out the Local On-Disk Cache

It should be full again, just like after workshop 1.

```
ls -al $GOPATH/pkg/mod
```

# 10 - Access an Athens Server Locally (Bonus)

## Setup

I'm going to start up an Athens server on my local machine using this command:

```
docker run --rm \
-p 3000:3000 \
-v $PWD/athens_storage:/athens \
-e ATHENS_STORAGE_TYPE=disk \
-e ATHENS_DISK_STORAGE_ROOT=/athens \
gomods/athens:v0.2.0
```

>Note: if you have [Docker](https://docker.com) installed on your local machine, you can run this command too and run your own Athens server

Next, I'm going to fire up an [ngrok](https://ngrok.com) tunnel so that you can access it over the internet. I'll use this command:

```
ngrok http 3000
```

## Steps for You

1. First, follow steps (3) and (5) above to remove your global on-disk cache and `hugo` binary
2. Next, set `GOPROXY` to the ngrok address
3. Finally, run `go build`

Things to notice:

- All those log messages that the local Athens spits out
- Athens is downloading dependencies behind the scenes and storing them in its on-disk database

## Bottom Line

**We've made Athens our cache (and our `vendor/` :grinning:) in the cloud! :cloud:**
