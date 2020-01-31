# DataFrames

In Julia dataframes are handled by an addon package. It can be installed with:

```julia
Pkg.add("DataFrames")
```
it's very likely that you will want to read and write dataframes as CSV files at some stage. This functionality is provided by the `CSV.jl` package, so you should install it as well:

```julia
Pkg.add("CSV")
```
to use the `DataFrames` and `CSV` packages you need to load them:

```julia
using CSV
using DataFrames
```
	
For the examples we'll use the `iris` dataset from the `RDatasets` package, so
we'll need to install it first if it's not already installed:
      

```julia
Pkg.add("RDatasets")
```
we can now load it and load the `iris` dataset:
   

```julia
using RDatasets
iris = dataset("datasets", "iris")
```

```
## 150×5 DataFrame
## │ Row │ SepalLength │ SepalWidth │ PetalLength │ PetalWidth │ Species      │
## │     │ Float64     │ Float64    │ Float64     │ Float64    │ Categorical… │
## ├─────┼─────────────┼────────────┼─────────────┼────────────┼──────────────┤
## │ 1   │ 5.1         │ 3.5        │ 1.4         │ 0.2        │ setosa       │
## │ 2   │ 4.9         │ 3.0        │ 1.4         │ 0.2        │ setosa       │
## │ 3   │ 4.7         │ 3.2        │ 1.3         │ 0.2        │ setosa       │
## │ 4   │ 4.6         │ 3.1        │ 1.5         │ 0.2        │ setosa       │
## │ 5   │ 5.0         │ 3.6        │ 1.4         │ 0.2        │ setosa       │
## │ 6   │ 5.4         │ 3.9        │ 1.7         │ 0.4        │ setosa       │
## │ 7   │ 4.6         │ 3.4        │ 1.4         │ 0.3        │ setosa       │
## ⋮
## │ 143 │ 5.8         │ 2.7        │ 5.1         │ 1.9        │ virginica    │
## │ 144 │ 6.8         │ 3.2        │ 5.9         │ 2.3        │ virginica    │
## │ 145 │ 6.7         │ 3.3        │ 5.7         │ 2.5        │ virginica    │
## │ 146 │ 6.7         │ 3.0        │ 5.2         │ 2.3        │ virginica    │
## │ 147 │ 6.3         │ 2.5        │ 5.0         │ 1.9        │ virginica    │
## │ 148 │ 6.5         │ 3.0        │ 5.2         │ 2.0        │ virginica    │
## │ 149 │ 6.2         │ 3.4        │ 5.4         │ 2.3        │ virginica    │
## │ 150 │ 5.9         │ 3.0        │ 5.1         │ 1.8        │ virginica    │
```
	
let's create a numeric vector and a string vector of the same length, and then put them in a dataframe:


```julia
y = rand(10)
```

```
## 10-element Array{Float64,1}:
##  0.35980655374348225
##  0.30914613783299005
##  0.7305710415910236 
##  0.28772472645844993
##  0.7304237025238804 
##  0.8263018940027678 
##  0.3042506763452575 
##  0.7658358345495233 
##  0.4057944014256023 
##  0.8265083273898153
```

```julia
cnd = [["a" for i=1:5]; ["b" for i=1:5]]
```

```
## 10-element Array{String,1}:
##  "a"
##  "a"
##  "a"
##  "a"
##  "a"
##  "b"
##  "b"
##  "b"
##  "b"
##  "b"
```

```julia
resFrame =  DataFrame(y=y, cnd=cnd)
```

```
## 10×2 DataFrame
## │ Row │ y        │ cnd    │
## │     │ Float64  │ String │
## ├─────┼──────────┼────────┤
## │ 1   │ 0.359807 │ a      │
## │ 2   │ 0.309146 │ a      │
## │ 3   │ 0.730571 │ a      │
## │ 4   │ 0.287725 │ a      │
## │ 5   │ 0.730424 │ a      │
## │ 6   │ 0.826302 │ b      │
## │ 7   │ 0.304251 │ b      │
## │ 8   │ 0.765836 │ b      │
## │ 9   │ 0.405794 │ b      │
## │ 10  │ 0.826508 │ b      │
```

to access the columns of a dataframe use the following syntax:


```julia
resFrame[!,:y] #retrieves the column named `y`
resFrame[!,:cnd] #retrieves the column named `cnd`
```

note that `:y` or `:cnd` are *Symbols*:


```julia
typeof(:y)
```

```
## Symbol
```

sometimes it is necessary to access the column of a dataframe using a string. You can do so by converting a string to a symbol:


```julia
resFrame[!,Symbol("y")]
```

you can also retrieve a column by its column number


```julia
resFrame[!,1]
```

to write a dataframe to a text of CSV file, use:


```julia
CSV.write("dataframe.csv", resFrame)
```

it is also possible to specify a separator, note that the separator character must be enclosed by
single ticks `''`:


```julia
CSV.write("dataframe.csv", resFrame, delim=';')
```

To read a dataframe from a text file, use:

```julia
dat = CSV.read("dataframe.csv", delim=';')
```


### Displaying data on screen

To show the first few rows of a dataframe on screen you can use the `first`
function, which takes as arguments the name of the dataframe and the number of
rows that you want to show:
     	 

```julia
first(iris, 4)
```

```
## 4×5 DataFrame
## │ Row │ SepalLength │ SepalWidth │ PetalLength │ PetalWidth │ Species      │
## │     │ Float64     │ Float64    │ Float64     │ Float64    │ Categorical… │
## ├─────┼─────────────┼────────────┼─────────────┼────────────┼──────────────┤
## │ 1   │ 5.1         │ 3.5        │ 1.4         │ 0.2        │ setosa       │
## │ 2   │ 4.9         │ 3.0        │ 1.4         │ 0.2        │ setosa       │
## │ 3   │ 4.7         │ 3.2        │ 1.3         │ 0.2        │ setosa       │
## │ 4   │ 4.6         │ 3.1        │ 1.5         │ 0.2        │ setosa       │
```

similarly to show the last few columns you can use the `last` command:
	  

```julia
last(iris, 4)
```

```
## 4×5 DataFrame
## │ Row │ SepalLength │ SepalWidth │ PetalLength │ PetalWidth │ Species      │
## │     │ Float64     │ Float64    │ Float64     │ Float64    │ Categorical… │
## ├─────┼─────────────┼────────────┼─────────────┼────────────┼──────────────┤
## │ 1   │ 6.3         │ 2.5        │ 5.0         │ 1.9        │ virginica    │
## │ 2   │ 6.5         │ 3.0        │ 5.2         │ 2.0        │ virginica    │
## │ 3   │ 6.2         │ 3.4        │ 5.4         │ 2.3        │ virginica    │
## │ 4   │ 5.9         │ 3.0        │ 5.1         │ 1.8        │ virginica    │
```

If you have a dataframe with too many columns to fit the screen some will be truncated. To get around this you can use:
   

```julia
##create dataframe with lost of columns
df = DataFrame((Symbol('a' + i) => rand(10) for i in 0:100)...)
show(df, allcols=true)
```
		  
## Selecting rows

We've already seen how we can select a given column of a dataframe. Selecting
rows of a dataframe on the basis of one or more conditions is another fundamental
operation. The following command returns a boolean vector indicating whether 
each element in the `Species` column of the iris datasets is "setosa" or not:


```julia
idx = iris[!,:Species] .== "setosa"
```

```
## 150-element BitArray{1}:
##  1
##  1
##  1
##  1
##  1
##  1
##  1
##  1
##  1
##  1
##  ⋮
##  0
##  0
##  0
##  0
##  0
##  0
##  0
##  0
##  0
```

we can use this vector to select the corresponding rows of the dataframe:
   	

```julia
iris[idx, :];
```

or more succintly we could achieve the same result with a single command:
   

```julia
iris[(iris[!,:Species] .== "setosa"), :];
```

we can also setect rows on the basis of multiple conditions, for example
only the ones for the setosa specimens with a petal length greater than 1.5:
     	

```julia
iris[(iris[!,:Species] .== "setosa") .& (iris[!,:PetalLength] .> 1.5), :]	    
```

```
## 13×5 DataFrame
## │ Row │ SepalLength │ SepalWidth │ PetalLength │ PetalWidth │ Species      │
## │     │ Float64     │ Float64    │ Float64     │ Float64    │ Categorical… │
## ├─────┼─────────────┼────────────┼─────────────┼────────────┼──────────────┤
## │ 1   │ 5.4         │ 3.9        │ 1.7         │ 0.4        │ setosa       │
## │ 2   │ 4.8         │ 3.4        │ 1.6         │ 0.2        │ setosa       │
## │ 3   │ 5.7         │ 3.8        │ 1.7         │ 0.3        │ setosa       │
## │ 4   │ 5.4         │ 3.4        │ 1.7         │ 0.2        │ setosa       │
## │ 5   │ 5.1         │ 3.3        │ 1.7         │ 0.5        │ setosa       │
## │ 6   │ 4.8         │ 3.4        │ 1.9         │ 0.2        │ setosa       │
## │ 7   │ 5.0         │ 3.0        │ 1.6         │ 0.2        │ setosa       │
## │ 8   │ 5.0         │ 3.4        │ 1.6         │ 0.4        │ setosa       │
## │ 9   │ 4.7         │ 3.2        │ 1.6         │ 0.2        │ setosa       │
## │ 10  │ 4.8         │ 3.1        │ 1.6         │ 0.2        │ setosa       │
## │ 11  │ 5.0         │ 3.5        │ 1.6         │ 0.6        │ setosa       │
## │ 12  │ 5.1         │ 3.8        │ 1.9         │ 0.4        │ setosa       │
## │ 13  │ 5.1         │ 3.8        │ 1.6         │ 0.2        │ setosa       │
```

The `DataFramesMeta` package also provides convenient functions for selecting rows
of a dataframes on the basis of one or more conditions.


```julia
using DataFramesMeta
@where(iris, :Species .== "setosa")
```

```
## 50×5 DataFrame
## │ Row │ SepalLength │ SepalWidth │ PetalLength │ PetalWidth │ Species      │
## │     │ Float64     │ Float64    │ Float64     │ Float64    │ Categorical… │
## ├─────┼─────────────┼────────────┼─────────────┼────────────┼──────────────┤
## │ 1   │ 5.1         │ 3.5        │ 1.4         │ 0.2        │ setosa       │
## │ 2   │ 4.9         │ 3.0        │ 1.4         │ 0.2        │ setosa       │
## │ 3   │ 4.7         │ 3.2        │ 1.3         │ 0.2        │ setosa       │
## │ 4   │ 4.6         │ 3.1        │ 1.5         │ 0.2        │ setosa       │
## │ 5   │ 5.0         │ 3.6        │ 1.4         │ 0.2        │ setosa       │
## │ 6   │ 5.4         │ 3.9        │ 1.7         │ 0.4        │ setosa       │
## │ 7   │ 4.6         │ 3.4        │ 1.4         │ 0.3        │ setosa       │
## ⋮
## │ 43  │ 4.4         │ 3.2        │ 1.3         │ 0.2        │ setosa       │
## │ 44  │ 5.0         │ 3.5        │ 1.6         │ 0.6        │ setosa       │
## │ 45  │ 5.1         │ 3.8        │ 1.9         │ 0.4        │ setosa       │
## │ 46  │ 4.8         │ 3.0        │ 1.4         │ 0.3        │ setosa       │
## │ 47  │ 5.1         │ 3.8        │ 1.6         │ 0.2        │ setosa       │
## │ 48  │ 4.6         │ 3.2        │ 1.4         │ 0.2        │ setosa       │
## │ 49  │ 5.3         │ 3.7        │ 1.5         │ 0.2        │ setosa       │
## │ 50  │ 5.0         │ 3.3        │ 1.4         │ 0.2        │ setosa       │
```
	

```julia
@where(iris, :Species .== "setosa", :PetalLength .> 1.5)
```

```
## 13×5 DataFrame
## │ Row │ SepalLength │ SepalWidth │ PetalLength │ PetalWidth │ Species      │
## │     │ Float64     │ Float64    │ Float64     │ Float64    │ Categorical… │
## ├─────┼─────────────┼────────────┼─────────────┼────────────┼──────────────┤
## │ 1   │ 5.4         │ 3.9        │ 1.7         │ 0.4        │ setosa       │
## │ 2   │ 4.8         │ 3.4        │ 1.6         │ 0.2        │ setosa       │
## │ 3   │ 5.7         │ 3.8        │ 1.7         │ 0.3        │ setosa       │
## │ 4   │ 5.4         │ 3.4        │ 1.7         │ 0.2        │ setosa       │
## │ 5   │ 5.1         │ 3.3        │ 1.7         │ 0.5        │ setosa       │
## │ 6   │ 4.8         │ 3.4        │ 1.9         │ 0.2        │ setosa       │
## │ 7   │ 5.0         │ 3.0        │ 1.6         │ 0.2        │ setosa       │
## │ 8   │ 5.0         │ 3.4        │ 1.6         │ 0.4        │ setosa       │
## │ 9   │ 4.7         │ 3.2        │ 1.6         │ 0.2        │ setosa       │
## │ 10  │ 4.8         │ 3.1        │ 1.6         │ 0.2        │ setosa       │
## │ 11  │ 5.0         │ 3.5        │ 1.6         │ 0.6        │ setosa       │
## │ 12  │ 5.1         │ 3.8        │ 1.9         │ 0.4        │ setosa       │
## │ 13  │ 5.1         │ 3.8        │ 1.6         │ 0.2        │ setosa       │
```

### Summarizing data

Summarizing data with the functions providing in the `DataFrames` package is
somwhat verbose:	   
	    

```julia
using Statistics	
summIris = by(iris, [:Species], df -> DataFrame(meanPetalWidth=mean(df[!,:PetalWidth])))
```

```
## 3×2 DataFrame
## │ Row │ Species      │ meanPetalWidth │
## │     │ Categorical… │ Float64        │
## ├─────┼──────────────┼────────────────┤
## │ 1   │ setosa       │ 0.246          │
## │ 2   │ versicolor   │ 1.326          │
## │ 3   │ virginica    │ 2.026          │
```
	
using the macros in `DataFramesMeta` is much better


```julia
summIris = @linq iris |> groupby(:Species) |> based_on(x = mean(:PetalWidth))
```

```
## 3×2 DataFrame
## │ Row │ Species      │ x       │
## │     │ Categorical… │ Float64 │
## ├─────┼──────────────┼─────────┤
## │ 1   │ setosa       │ 0.246   │
## │ 2   │ versicolor   │ 1.326   │
## │ 3   │ virginica    │ 2.026   │
```

there are slightly different ways of achieving the same result:


```julia
@linq iris |> by(:Species, x=mean(:SepalWidth))
```

```
## 3×2 DataFrame
## │ Row │ Species      │ x       │
## │     │ Categorical… │ Float64 │
## ├─────┼──────────────┼─────────┤
## │ 1   │ setosa       │ 3.428   │
## │ 2   │ versicolor   │ 2.77    │
## │ 3   │ virginica    │ 2.974   │
```
	  	
or:
		

```julia
@based_on(groupby(iris, :Species), x=mean(:PetalWidth))		 	
```

```
## 3×2 DataFrame
## │ Row │ Species      │ x       │
## │     │ Categorical… │ Float64 │
## ├─────┼──────────────┼─────────┤
## │ 1   │ setosa       │ 0.246   │
## │ 2   │ versicolor   │ 1.326   │
## │ 3   │ virginica    │ 2.026   │
```
		
