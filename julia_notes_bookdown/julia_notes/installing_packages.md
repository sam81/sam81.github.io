# Installing and managing packages {#packageManagement}

There are two ways of installing and managing packages in Julia. One is through functions at the Julia REPL, the other is through the *package manager REPL*. For example, to install the `Gadfly` package from the Julia REPL you can use the `Pkg.add` function:


```julia
Pkg.add("Gadfly")
```

to do the same from the package manager REPL you need to first access the package manager REPL by typing `]` in the Julia REPL. Once you've entered the package manager REPL, which should show `pkg>` in the prompt, you can install a package using the `add` command:


```julia
add Gadfly
```

Passing the name of the package to install, as shown in the examples above works for packages registered in the Julia package database. If you want to install unregistered packages that are available on GitHub or other git repositories you need to pass the git URL of the package that you want to install. For example, you can install the [SndLib.jl](https://github.com/sam81/SndLib.jl) package from the Julia REPL as follows:


```julia
Pkg.add("git@github.com:sam81/SndLib.jl.git")
```

To update your packages to their latest version you can use the `Pkg.update()` command from the julia REPL, or the `up` command from the package manager REPL.

To remove a package you can use the `Pkg.rm("packageName")` command from the Julia REPL, or the `rm packageName` command from the package manager REPL.
