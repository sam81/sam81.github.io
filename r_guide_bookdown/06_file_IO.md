# File Input/Output {#IO}



## Reading In Data from a File

### The `read.table` function {#read.table}

If the data are in a table-like format, with each row corresponding to an observation or a single case, and each column to a variable, the most convenient function to load them in R is `read.table`. This function reads in the data file as a dataframe. For example the data in the data file `ratsData.txt` that contain information (height, weight, and species) of 6 rats can be easily read in with the following command:

```r
ratsData = read.table(file="ratsData.txt", header=TRUE)
ratsData
    identifier height weight species
  1       sa01    3.2    300       A
  2       sa02    2.6    246       A
  3       sa03    2.9    317       A
  4       sb01    2.4    229       B
  5       sb02    2.5    230       B
  6       sb03    2.4    245       B
```
in this case we've set the option `header=TRUE` because the file contains a header on the first line with the variable names.


### `scan` {#scan}

Another very handy function for reading in data is `scan`, it can easily read in both tabular data in which all the columns are of the same type, or they are of different type, as long as they follow a regular pattern. We'll read in the `ratsData.txt` file as an example:

```r
x = scan(file="ratsData.txt", what=list(identifier=character(),
          height=numeric(), weight=numeric(), species=character()),
          skip=1)
  Read 6 records
str(x)
  List of 4
   $ identifier: chr [1:6] "sa01" "sa02" "sa03" "sb01" ...
   $ height    : num [1:6] 3.2 2.6 2.9 2.4 2.5 2.4
   $ weight    : num [1:6] 300 246 317 229 230 245
   $ species   : chr [1:6] "A" "A" "A" "B" ...
```
the `what` argument tells the `scan` function the mode (numeric, character, etc...) of the elements to be read in, if `what` is a list of modes, then each corresponds to a column in the data file. So in the above example we have the first column, the variable `identifier`, which is of character mode, the second column, `height` is of numeric mode, like the third column, `weight`, while the last column, `species` is of character mode again. Notice that we're telling `scan` to skip the first line of the file (`skip=1`) because it contains the header. The object returned by `scan` in this case is a list, we can get the single elements of the list, corresponding to each column of the data file, with the usual methods for lists:

```r
x[[1]] #first item in list (first column in file)
  [1] "sa01" "sa02" "sa03" "sb01" "sb02" "sb03"
x[["weight"]] #item named weight in list (third column in file)
  [1] "300" "246" "317" "229" "230" "245"
```

`scan` can do much more than what was shown in this example, like specifying the separator between the fields of the data file (comma, tabs, or whatever else, defaults to blank space), or specifying the maximum number of lines to be read, see `?scan` for more details. I'll present just another simple example in which we'll read in a file whose data are all numeric. The file is `rts.txt`

```r
x = scan("rts.txt", what=numeric())
  Read 36 items
str(x)
   num [1:36] 0.12 0.132 0.102 0.096 0.103 0.087 0.113 ...
```

in this case the object returned is a long vector of the same mode as the `what` argument, a numeric vector. The file is however organised into three columns, which represent three different numeric variables. It is easy to reorganise our vector to reflect the structure of our data:

```r
nRows = length(x)/3
xm = matrix(x, nrow=nRows, byrow=TRUE)
xm
         [,1]  [,2]  [,3]
   [1,] 0.120 0.132 0.102
   [2,] 0.096 0.103 0.087
   [3,] 0.113 0.134 0.109
   [4,] 0.132 0.147 0.123
   [5,] 0.124 0.139 0.124
   [6,] 0.105 0.115 0.102
   [7,] 0.109 0.129 0.097
   [8,] 0.143 0.150 0.119
   [9,] 0.127 0.145 0.113
  [10,] 0.098 0.117 0.092
  [11,] 0.115 0.126 0.098
  [12,] 0.117 0.132 0.103
```

so we've got a matrix with the 3 columns of data originally found in the file, turning it into a dataframe would be equally easy at this point

```r
xd = as.data.frame(xm)
```

However also in this case it is possible to simply read in each column of the file as the element of a list:

```r
x = scan("rts.txt", what=list(v1=numeric(),
          v2=numeric(), v3=numeric()))
  Read 12 records
str(x)
  List of 3
   $ v1: num [1:12] 0.12 0.096 0.113 0.132 0.124 0.105 0.109 ...
   $ v2: num [1:12] 0.132 0.103 0.134 0.147 0.139 0.115 0.129 ...
   $ v3: num [1:12] 0.102 0.087 0.109 0.123 0.124 0.102 0.097 ...
```

### Low-Level File Input

Sometimes the file to be read is not nicely organised into separate columns each representing a variable, in this case the function `readLines` can either read the file one line at the time, or read all the lines at once. The lines can then be further processed to extract the data.

## Writing data to a file

### The `write.table` function {#write.table}

The function `write.table` provides a simple interface for writing data to a file. It can be used to write a dataframe or a matrix to a file. For example, if `mydats` is a dataframe, the command

```r
write.table(mydats, file="my_data.txt",
                    col.names=TRUE, row.names=FALSE)
```
will store it in the text file `my_data.txt` with the labels for the variables or factors it contains in the first row(`col.names=T`), but without the numbers associated with each row (`row.names=F)`. The `sep` option is used to choose the separator for the data, the default is a blank space `sep=" "`, but you can choose a comma `sep=","` a semicolon `sep=";"` or other meaningful separators.

### Saving objecs in binary format 

Probably the most convenent function to save R objects (dataframes, lists, matricies, etc...) in binary format is `saveRDS`:

```r
saveRDS(iris, file="iris.rds")
```

the `readRDS` function can be used to read back the object into R:

```r
iris_dset = readRDS("iris.rds")
```

### Low-Level File Output {#cat}

Perhaps the most user friendly low-level function for writing to a file is `cat`. Suppose you have two vectors, one with the heights of 5 individuals, and one with an identifier for each, and you want to write these data to a file:

```r
height = c(176, 180, 159, 156, 183)
id = c("s1", "s2", "s3", "s4", "s5")
```
you can use `cat` in a `for` loop to write the data to the file, but let's first write a header

```r
cat("id height \n", file="foo.txt", sep=" ", append=FALSE)
```
the first argument is the object to write, in this case a character string with the names of our variables to make a header, and a newline (`\n') character to start a new line. Now the `for` loop:

```r
for (i in 1:length(id)){
  cat(id[i], height[i], "\n", file="foo.txt",
      sep=" ", append=TRUE)}
```
note that this time we've set `append=TRUE` to avoid overwriting both the header, and any previous output from the preceding cycle in the for loop. We've been using a blank space as a separator, but we could have used something else, for example a comma (`sep=","`).

While using `cat` to automatically open and close a file as we did above is convenient, when we need to repeatedly write to the same file it's more efficient to explicitly open a file connection first, write to it, and then close it as shown below:


```r
fHandle = file("test_write.txt", "w")

cat("id height \n", file=fHandle, sep=" ")
for (i in 1:length(id)){
    cat(id[i], height[i], "\n", file=fHandle,
        sep=" ")
}

close(fHandle)
```


