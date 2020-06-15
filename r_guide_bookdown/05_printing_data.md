# Printing out data on the console

As you probably have already noted, after you've created a data object, just typing its name on the console and pressing `Enter` will display the values it contains. Sometimes however, you might want to see only part of the data, for example to do some checking, or because the data object is too big and it's not printed nicely on the console. The functions `head` and `tail` let you look only at the first or the last part of your data respectively. For example, let's load the `iris` dataset in the `datasets` package, the command:

```r
library(datasets)
head(iris)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```
will print only the first 6 observations. You can visualise more (or less) than 6 observations by setting the `n` option:

```r
head(iris, n=10)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1           5.1         3.5          1.4         0.2  setosa
## 2           4.9         3.0          1.4         0.2  setosa
## 3           4.7         3.2          1.3         0.2  setosa
## 4           4.6         3.1          1.5         0.2  setosa
## 5           5.0         3.6          1.4         0.2  setosa
## 6           5.4         3.9          1.7         0.4  setosa
## 7           4.6         3.4          1.4         0.3  setosa
## 8           5.0         3.4          1.5         0.2  setosa
## 9           4.4         2.9          1.4         0.2  setosa
## 10          4.9         3.1          1.5         0.1  setosa
```

## Reading numbers in exponential notation

Often R prints out numbers in exponential notation. In order to understand exponential notation it's first necessary to introduce *scientific notation*.

 A number in scientific notation is in the form $a\cdot 10^b$, for example 300 could be written in scientific notation as $3\cdot 10^2$. The components of a number in scientific notation are also named as $mantissa \cdot 10^{characteristic}$. Remember that a number with a negative exponent, for example $10^{-2}$ can be rewritten as
\[10^{-2}= \frac{1}{10^2}=0.01\]
so, for example, $0.003$ can be rewritten in scientific notation as $3\cdot 10^{-3}$, because
\[3\cdot 10^{-3}= 3\cdot \frac{1}{10^3}= 0.003\]

R, as most calculators doesn't actually use scientific notation, it uses instead *exponential* notation. The exponential notation is a shorthand version of the scientific notation, in which, for example, $10^3$ is replaced by $e3$, where $e$ stands for *exponent*. So in our previous examples 300 would be written as $3e2$ and 0.003 would be written as $3e3$. Below are a few conversion examples.

Number    Scientific Notation  Exponential Notation
------    -------------------  --------------------
$10$      $1\cdot 10^1$        $1e1$
$20$      $2\cdot 10^1$        $2e1$
$200$     $2\cdot 10^2$        $2e2$
$350$     $3.5\cdot 10^2$      $3.5e2$
$0.00353$ $3.53\cdot 10^-3$    $3.53e-3$

Table: (\#tab:numberformat) Number Formats.

As a quick and dirty rule, remember that when you're multiplying a number by $10^{exponent}$, as you add to the exponent, you're adding a 0 to the number, or shifting the point by one position towards the right if it's a decimal number. As you subtract to the exponent, you're deleting a 0 to the number, or shifting the point by one position towards the left.


