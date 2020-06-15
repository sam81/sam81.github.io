# Installing R {#intro}

```{=latex}
\pagenumbering{arabic}
```



The information provided in this section is quite generic and limited. For detailed information please refer to the "R Installation and Administration Manual" available at the CRAN website https://cran.r-project.org. CRAN stands for "Comprehensive R Archive Network", and is the main point of reference for the R software. There you can find R sources, binaries, documentation, and add-on packages.

## Installing the base program

First of all you need to install the base R program. There are two ways of doing this, you can either compile the source code yourself, or you can install the precompiled binaries for your specific operating system. The second way is the easiest one, and usually you will want to go with it.

### GNU/Linux and other Unix systems

Precompiled binaries are available for some GNU/Linux distributions, there is a list of these on the [R FAQ](https://cran.r-project.org/doc/FAQ/R-FAQ.html). For other distributions you can build R from source. There are precompiled binaries for Debian GNU/Linux, so you can install the R base system and a number of add-on packages with the usual methods under Debian, that is, by using `apt-get`, or a graphical package manager such as `Synaptic`, or whatever else you normally use.

### Windows

There are precompiled binaries for Windows, you just have to download them, then double click on the installer's icon to start the installation. Most Windows versions are supported.

### Mac Os X

Precompiled binaries are also available for Mac Os X, you can download them and then double click on the installer's icon to start the installation.

## Installing add-on packages

There is a vast number of packages that implement statistical functions that are not available with the base program. They're not strictly necessary, but if you keep using R, sooner or later you will want to install some of these packages.

### GNU/Linux and other Unix systems

There are different ways to get packages installed. For Debian there are precompiled versions of some packages, so you can get them with `apt-get` or whatever else you usually use to install Debian packages; this will also take care of possible dependencies (some packages need other packages or system libraries to be installed in order to work). For other packages, from an R session you can call the `install.packages` function, for example:

```r
install.packages("gplots")
```
will install the `gplots` package. You can also install more than one package in one go:

```r
install.packages(c("gplots", "signal"))
```
will install the `gplots` and `signal` packages. Another way to install a package is to download the related tarball from CRAN and then from a console issue the command:

	R CMD INSTALL /packagename

where `packagename` is the full path to the tarball you have downloaded. There are other ways and other specific options to install add-on packages. You should refer to the "R Installation and Administration Manual" for further information. 

### Windows

The R graphical user interface (GUI) on Windows provides an interface to download and install the packages directly from the internet. 


