# Random Numbers and Sampling

One of the fundamental functions to generate random number and random sampling in Julia is `rand`. This function picks random elements from a collection of items. If the function is called without arguments it will generate a random 64-bit floating point number between 0 and 1 because that is the default collection:


```julia
rand()
```

```
## 0.4023878488048531
```

to generate a random integer between say 1 and 100 we need to pass that collection as an argument:


```julia
rand(1:100)
```

```
## 85
```

the collection to sample from could be something different than numbers. For example we can generate a sequence of letters and sample from it:


```julia
lettersSeq = ["a", "b", "c", "d"];
rand(lettersSeq)
```

```
## "a"
```

to generate more than one element we need to specify the dimensions of the vector or matrix of elements that we want to generate:


```julia
#generate a 10-elements vector of
#random integers between 1 and 100
rand(1:100, 10)
```

```
## 10-element Array{Int64,1}:
##  84
##  58
##  89
##  82
##  32
##  14
##  85
##  26
##  72
##  61
```

```julia
#generate a 2x5 matrix of
#random integers between 1 and 100
rand(1:100, 2, 5) 
```

```
## 2×5 Array{Int64,2}:
##  95  13   6  83  57
##  93  79  80  76  81
```

note that sampling is done with replacement:

```julia
rand(["a", "b", "c", "d"], 4)
```

```
## 4-element Array{String,1}:
##  "d"
##  "b"
##  "c"
##  "b"
```

```julia
rand(["a", "b", "c", "d"], 10)
```

```
## 10-element Array{String,1}:
##  "a"
##  "d"
##  "b"
##  "a"
##  "c"
##  "b"
##  "c"
##  "d"
##  "c"
##  "d"
```

the [StatsBase.jl](https://github.com/JuliaStats/StatsBase.jl) package provides convenient functions to sample with or without replacement. For example, to sample without replacement:


```julia
using StatsBase
sample(["a", "b", "c", "d"], 4, replace=false)
```

```
## 4-element Array{String,1}:
##  "b"
##  "d"
##  "c"
##  "a"
```

	
## Setting the random seed

It is sometimes desirable that a call to a random number generator actually generates the same "random" sequence each time it is called at a specific point in a script (e.g. to make an example reproducible). To achieve this it is necessary to explicitly initialize the random number generator with a ["seed"](https://en.wikipedia.org/wiki/Random_seed). In julia this can be done with the `Random.seed!` function in the standard library package `Random`:
   

```julia
import Random
Random.seed!(375)
```

## Working with distributions

The `Distributions.jl` package provides functions to sample from various distributions. For example, to sample from a normal distribution with mean=10, and sd=2, we first instantiate it:


```julia
using Distributions
d = Normal(10, 2)
```

```
## Normal{Float64}(μ=10.0, σ=2.0)
```

then we sample from it with `rand`:


```julia
rand(d, 10)
```

```
## 10-element Array{Float64,1}:
##  13.097676249974274
##  11.757335823480798
##  12.237501126505748
##  13.168175635876224
##  11.14411361628477 
##  10.67240877975671 
##  10.150159377468727
##   5.931032863139524
##  11.139710601976821
##  11.021572604999648
```

we would do the same more succintly with:


```julia
rand(Normal(10,2), 10)
```

To sample from a uniform distribution:


```julia
rand(Uniform(20, 30), 5)
```

```
## 5-element Array{Float64,1}:
##  24.767443081616506
##  26.203224039053918
##  23.73620344244641 
##  29.20225681258401 
##  21.679924712717167
```
