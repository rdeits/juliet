# Juliet: More powerful Julia package installation

## Installation

Just copy or symlink the `juliet` script somewhere on your `PATH`.

## Background and Usage

Julia is able to install packages by reading from a `REQUIRE` file, which specifies the name and desired version number or version range of each package to be installed. However, the `REQUIRE` spec has a few limitations:

* packages must already be registered in `METADATA.jl`
* versions must correspond to tagged releases in `METADATA.jl`

It's easy to install non-registered Julia packages within Julia by using `Pkg.clone()`, and it's easy to set a package to a particular git branch with `Pkg.checkout()`. But if your package depends on a particular commit of an unregistered package, then managing that dependency can be difficult.

Juliet tries to make this easier. To use Juliet, you should create a development requirements file, which we call `REQUIRE.dev` by default. `REQUIRE.dev` supports the standard `REQUIRE` file syntax, but it *also* allows any line to consist of a single git repository URL and a single git commit ref (a sha, tag, or branch name). For example:

### REQUIRE.dev:

```
IJulia
https://github.com/rdeits/DrakeVisualizer.jl.git 0766c8c93
https://github.com/JuliaLang/Reactive.jl.git v0.3.3
Interact 0.3.1
```

Running `juliet install` will use Julia's `Pkg` module to clone (if necessary) and build the specified revisions of all the `REQUIRE.dev`. Since `Pkg` already understands the dependencies of cloned packages, this will also recursively install all the dependencies of those packages as well.

By default, this means that packages will be installed to your global Julia package directory (typically `~/.julia`). To avoid affecting your global state, you may want to create a local package directory. To do that, just make sure you set the `JULIA_PKGDIR` environment variable before running `juliet` and make sure it's still set when you run `julia` itself:

```bash
cd ~/projects/my_cool_project
export JULIA_PKGDIR=$HOME/projects/my_cool_project/packages
juliet install
julia
```
