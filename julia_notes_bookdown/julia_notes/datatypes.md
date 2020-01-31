# Data Types

## A quick overview of the most common data types

Julia has a sophisticated type system, and a variety of built-in data types and structures. If you're totally new to programming you may want to learn the basics of how numbers are represented in computers (e.g. learn the difference between integers and floating point numbers) because a basic knowledge of these topics will be very useful, if not essential, when working with Julia. 

You can check the type of an object with the `typeof` function. For example:


```julia
typeof(3) ## this is an integer
```

```
## Int64
```


```julia
typeof(3.0) ## this is a floating point number
```

```
## Float64
```

You can convert easily between the two:


```julia
x = 3
```

```
## 3
```

```julia
x = float(x) ##convert to floating point number
```

```
## 3.0
```


```julia
x = 3.0
```

```
## 3.0
```

```julia
x = round(Int, x) #convert to an integer
```

```
## 3
```

Now that we've seen some numbers let's move on to strings:


```julia
s = "Hello" #this is a string
```

```
## "Hello"
```

```julia
z = "3" #and this is also a string
```

```
## "3"
```

we can convert `z` above to an integer or a floating point number with the `parse` function:


```julia
parse(Int, z) #convert into an integer
```

```
## 3
```

```julia
parse(Float64, z) #convert to a floating point number
```

```
## 3.0
```
One of the most useful data structures are arrays. Arrays are containers that can contain objects of different types. We can initialize arrays of numbers with the `zeros` function:


```julia
x = zeros(5)
```

```
## 5-element Array{Float64,1}:
##  0.0
##  0.0
##  0.0
##  0.0
##  0.0
```

by default this initializes an array of 64-bit floating point numbers, but we can specify a different type if we want to:


```julia
x = zeros(Int, 5) #array of integers
```

```
## 5-element Array{Int64,1}:
##  0
##  0
##  0
##  0
##  0
```

```julia
x = zeros(Float32, 5) #array of 32-bit floats
```

```
## 5-element Array{Float32,1}:
##  0.0
##  0.0
##  0.0
##  0.0
##  0.0
```

we can also intialize arrays with more than one dimension:


```julia
x = zeros(3,3) #3x3 matrix
```

```
## 3×3 Array{Float64,2}:
##  0.0  0.0  0.0
##  0.0  0.0  0.0
##  0.0  0.0  0.0
```

```julia
x = zeros(3,3,3) #3x3x3 array
```

```
## 3×3×3 Array{Float64,3}:
## [:, :, 1] =
##  0.0  0.0  0.0
##  0.0  0.0  0.0
##  0.0  0.0  0.0
## 
## [:, :, 2] =
##  0.0  0.0  0.0
##  0.0  0.0  0.0
##  0.0  0.0  0.0
## 
## [:, :, 3] =
##  0.0  0.0  0.0
##  0.0  0.0  0.0
##  0.0  0.0  0.0
```

Arrays can also contain strings:


```julia
x = ["a", "b", "c"]
```

```
## 3-element Array{String,1}:
##  "a"
##  "b"
##  "c"
```

or be miscellaneous collections of objects of `Any` type (similar to Python lists):


```julia
x = [1, "a", 3.0]
```

```
## 3-element Array{Any,1}:
##  1   
##   "a"
##  3.0
```

including other arrays:


```julia
x = [1, "a", 3.0, [1,2,3]]
```

```
## 4-element Array{Any,1}:
##  1         
##   "a"      
##  3.0       
##   [1, 2, 3]
```

Dictionaries, consisting of key-value pairs, constitute anothe very useful object. The key and the values can be of different types. For example:


```julia
myDict = Dict{String, Any}()
```

will initialize a dictionary with a string key, and a value that can be of any type:


```julia
myDict["a"] = 3
myDict["b"] = "hello"
```

### Arrays

Initializing vectors of a given type:


```julia
x = (Int)[] #integer
```

```
## 0-element Array{Int64,1}
```

```julia
y = (Real)[] #floating point
```

```
## 0-element Array{Real,1}
```

```julia
z = (String)[] # string
```

```
## 0-element Array{String,1}
```

you can then `push!` elements inside the vectors


```julia
push!(x, 3)
```

```
## 1-element Array{Int64,1}:
##  3
```

```julia
push!(y, 3.2)
```

```
## 1-element Array{Real,1}:
##  3.2
```

```julia
push!(z, "abc")
```

```
## 1-element Array{String,1}:
##  "abc"
```

### Dictionaries

Nested dictionaries:


```julia
mydict = Dict{String, Any}()
```

```
## Dict{String,Any} with 0 entries
```

```julia
mydict["nest1"] = Dict{String, Any}()
```

```
## Dict{String,Any} with 0 entries
```

```julia
mydict["nest1"]["nest2"] = [1,2,3]
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```
