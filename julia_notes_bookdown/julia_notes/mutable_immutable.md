# Mutable and immutable objects

\BeginKnitrBlock{rmdwarning}<div class="rmdwarning">This topic can be somewhat mind-boggling! While I strive to write correct definitions and explanations, remember that these are simply my notes on using Julia, and I'm not an expert! So take my words and explanations with a grain of salt, 	and drop me an e-mail or fork these notes on github to fix them if you find errors.</div>\EndKnitrBlock{rmdwarning}
	

Objects in Julia can either be mutable or immutable, you can check whether
an object is immutable with the `isimmutable` function, for example, integers and
floating point numbers are immutable:


```julia
isimmutable(1)
```

```
## true
```


```julia
isimmutable(1.1)
```

```
## true
```

while arrays and dictionaries are mutable:


```julia
isimmutable([1,2,3])
```

```
## false
```


```julia
isimmutable(Dict("a" => 1, "b" => 2, "c" => 3))
```

```
## false
```

Immutable objects cannot be changed, while mutable objects can. Why is this important? Because they behave very differently. For example, suppose that you bind variable `a` to an immutable object:


```julia
a = 1
```

```
## 1
```

then you bind another variable, `b`, to `a`:


```julia
b = a
```

```
## 1
```

you can check that `a` and `b` are bound to the same object:


```julia
println(objectid(a))
```

```
## 11967854120867199718
```

```julia
println(objectid(b))
```

```
## 11967854120867199718
```

```julia
objectid(a) == objectid(b)
```

```
## true
```

now if you change the value of `a`, you're binding it to a new object, while `b` is still bound to the previous one:


```julia
a = 2
```

```
## 2
```

```julia
println(objectid(a))
```

```
## 5352850025288631388
```

```julia
println(b)
```

```
## 1
```

```julia
println(objectid(b))
```

```
## 11967854120867199718
```

so, `a` and `b` now have different values. So far so good, but what happens with variables bound to mutable objects? Let's try to bind `x` to an array, which is mutable, and then bind `y` to `x`:


```julia
x = [1,2,3]
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```

```julia
y = x
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```

```julia
println(objectid(x))
```

```
## 15596065500155535289
```

```julia
println(objectid(y))
```

```
## 15596065500155535289
```

now we change the value of the first element of `x`:


```julia
x[1] = 999
```

```
## 999
```

```julia
println(y)
```

```
## [999, 2, 3]
```

```julia
println(objectid(x))
```

```
## 15596065500155535289
```

```julia
println(objectid(y))
```

```
## 15596065500155535289
```

we can see that the first element of `y` has changed as well, and `x` and `y` are still bound to the same object. When we modify the value of the first element of `x` we're not binding `x` to a new object, we're modifying the old one, so `x` and `y` are still bound to the same object, and changing the value of the first element of `x` also changes the value of the first element of `y`. But watch out! If you do:


```julia
x = [1,2,3]
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```

```julia
y = x
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```

```julia
println(objectid(x))
```

```
## 7365922990070846502
```

```julia
println(objectid(y))
```

```
## 7365922990070846502
```

```julia
x = [999, 2, 3]
```

```
## 3-element Array{Int64,1}:
##  999
##    2
##    3
```

```julia
println(y)
```

```
## [1, 2, 3]
```

```julia
println(objectid(x))
```

```
## 12877331896388202487
```

```julia
println(objectid(y))
```

```
## 7365922990070846502
```

you're not modifying the mutable object to which `x` was bound, you're binding `x` to a totally new object! So `x` and `y` will be bound to different objects.

\BeginKnitrBlock{rmdtip}<div class="rmdtip">It may help to think of a mutable object as a container of items.</div>\EndKnitrBlock{rmdtip}
	
When you change an item in the container the binding of a variable to the container doesn't change, but if you change the container... the binding will change. Here's anothe example in which we change the container:


```julia
x = [1,2,3]
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```

```julia
y = x
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```

```julia
x = [1,2,3] #it looks the same but we're changing the container!
```

```
## 3-element Array{Int64,1}:
##  1
##  2
##  3
```

```julia
objectid(y) == objectid(x) #not the same object
```

```
## false
```

despite the fact that they have the same value:

```julia
x==y
```

```
## true
```

All this may seem abstract and, and may seem to be touching edge cases, but it has important consequences. One of these is that if you pass a mutable object to a function as an argument, and you modify that object inside the function, the object will be modified even though you may not explicitly return it. For example, suppose that we write a function to sum two arrays, and in case the first array is longer than the second one we remove elements from it until their length matches:


```julia
function foo(x,y)
    while length(x) > length(y)
        pop!(x)
    end
    z = x+y
    return z
end    
```

note that the function does not explicitly return `x`, but will nontheless modify it. Now let's call the function with the following arguments:


```julia
x = [1,1,1,1];
y = [2,2];
z = foo(x, y)
```

```
## 2-element Array{Int64,1}:
##  3
##  3
```

we get the value of `z` which we explicitly returned, but if we check what value now `x` has:


```julia
x
```

```
## 2-element Array{Int64,1}:
##  1
##  1
```

we can see that it has been modified as well. This doesn't happen with immutable objects. Let's check it with another very contrived example:


```julia
function foo(x,y)
    while x > y
        x = x-1
    end
    z = x+y
    return z
end    
```


```julia
x = 8; y=3;
foo(x, y)
```

```
## 6
```

the function returns the expected value, and if we check what's the value of `x`:


```julia
x
```

```
## 8
```

we can see that it has not changed. That's because `x` is bound to an immutable object in this case, and the variable `x` inside the function is in a different scope from the variable `x` outside the function.

\BeginKnitrBlock{rmdimportant}<div class="rmdimportant">The moral of the story above is: Watch out when you pass mutable objects as function arguments!</div>\EndKnitrBlock{rmdimportant}
	
Another important side effect of the difference between mutable and immutable objects involves the local scope introduced by certain code blocks, such as `for` loops. Variables from the global scope are available for reading in the local scope, but not for writing. So for example the `x` in the `for` loop below will be bound to a new object in the local scope of the loop:


```julia
x = 0;
for i=1:3
    x = i
    println(x)
end
```

```
## 1
## 2
## 3
```

we can check that the `x` in the global scope has not been modified:


```julia
x
```

```
## 0
```

But if `x` is a mutable object, and we modify one of its elements inside the loop:


```julia
x = [0,0,0];
for i=1:3
    x[2] = i
    println(x)
end
```

```
## [0, 1, 0]
## [0, 2, 0]
## [0, 3, 0]
```

this will be reflected in the global scope:


```julia
x
```

```
## 3-element Array{Int64,1}:
##  0
##  3
##  0
```
