# Workflow

## Environment customization

You can customize your Julia environment by inserting code in the `startup.jl` file located in `~.julia/config/` (create both the directory tree and the file if they don't yet exist in your system), which is run each time Julia starts up. You could use this to load packages that you use frequently, define functions or set variables that you use often in your interactive sessions. Below is an example of a `startup.jl` file:


```julia
push!(LOAD_PATH, "/path/to/my/modules/")
using Pkg
```

the first line appends the directory path to local modules to the `LOAD_PATH` environment variable, so that they can be loaded if needed. The second line loads the `Pkg` module, something that may be useful if you frequently install/update packages on your system and find it annoying to load the module manually each time.
