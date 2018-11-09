# Athens

Go dependency management has come a long way. When the language was first open sourced, there was very
little support for versioned dependencies at all. Since then, we've seen several iterations of technologies that brought us to the new Go modules system. With modules we have a new dependency management system built right into the `go` tool, and all of us in the community have a standard way to fetch dependencies! This is great news :tada:

Modules are designed by the go team, and fit right into the `go` CLI. Fetching dependencies is a simple `go get` away, and you can check in a file next to your source code, and everyone else will build your code with the exact same dependencies that you did - without using external dependency manager tools. That's a first for Go!

We'll learn all about modules, and then do a hands-on coding session where we integrate modules into a real project.

Then, we'll dive into the next exciting technology called module proxies, which make dependency management even simpler and more reliable. We'll learn about proxies and get familiar with the Athens project - a community-built proxy. Then, we'll do a hands-on coding session where we integrate the Athens proxy into the project we used before.

You'll walk away from this workshop knowing:

- How to do proper dependency management for Go projects
- What to expect with modules
- How to integrate them with your projects
- Why module proxies matter and what they can do for you
- What Athens is and how to integrate it with your project
- How to get involved with Athens

