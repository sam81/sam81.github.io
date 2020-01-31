# A simple introduction to Julia

It is customary to start a tutorial on a programming language by printing the phrase "Hello world!". We can do that from a julia terminal (also known as REPL, which stands for read–eval–print loop) using the `println` function:


```julia
println("Hello world!")
```

```
## Hello world!
```

but tipically we want to save our programs in a script for later execution. So suppose we have saved our `println("Hello world!")` command in a file called `test_julia.jl`. How do we execute the program? We can use the `include` function for this purpose:


```julia
include("test_julia.jl")
```
 If you're new to programming, it may be helpful to think of julia as a fancy calculator initially, and using it as such to familiarize yourself with it. For example you can perform simple arithmetic operations from the REPL:
 

```julia
5+7
```

```
## 12
```

you can also assign the results of operations to variables, for example:


```julia
x = 5+7
```

```
## 12
```

and use these variables for further operations, such as:


```julia
x/3
```

```
## 4.0
```

Of course julia is much more than just a fancy calculator, and hopefully this tutorial (which is very much of a work in progress, and therefore very patchy) will give and idea of what you can do with julia and how.

If you're new to julia you should be aware that the basic installation only provides a limited set of core functions. Additional functionality is provided by addon packages which need to be installed and initialized in order to be used. More detailed info on installing and managing packages is available in Section \@ref(packageManagement). For a quickstart, suppose that you want to compute the mean of a set of numbers. To do this you need to install the `StatsBase` package first, which provides statistical functions. You can do this with the following command:


```julia
Pkg.install("StatsBase")
```

to use the functions in the package you need to load it into your workspace with the `using` command:


```julia
using StatsBase
```

now you can use the `mean` function, for example:


```julia
mean([1,2,3])
```

```
## 2.0
```

Julia has taken a very modular approach splitting the core language functions from domain specific functions which are provided by add on packages. For example a base installation of julia does not have built in plotting functionality. For this you need to install one of the plotting packages. An overview of some of these packages is provided in other sections of this document: [Gadfly](#gadfly), [PyPlot](#pyplot), [VegaLite](#vegalite). You can also call R from julia, and so you can indirectly use `ggoplot2` from julia. This is illustrated in the [ggplot2](#ggplot2) section.

## Getting help

To get help on a function you can type the question mark (`?`) at the REPL, followed by the name of the function for which you seek help, for example to get help on the `println` function you can type:


```julia
?println
```

