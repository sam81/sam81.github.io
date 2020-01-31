# Parallel Processing

The BLAS library automatically makes use of multiple cores in your machine. You can tell it how many threads to use with the `BLAS.set_num_threads` function in `LinearAlgebra`:


```julia
using LinearAlgebra
BLAS.set_num_threads(8)
```

