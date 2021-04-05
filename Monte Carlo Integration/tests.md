## easy-test

### Objective :
compile and run volesti. Read the CRAN package documentation, generate a random H-polytope and compute its volume.

`volesti` project compiled, build and run Succesfully : [build start](https://user-images.githubusercontent.com/49487927/112721557-78062880-8f2a-11eb-87f8-25d513bb4245.png), [build done](https://user-images.githubusercontent.com/49487927/112721554-763c6500-8f2a-11eb-9eff-05a7d6000edc.png), [testcase-passed](https://user-images.githubusercontent.com/49487927/112721558-79375580-8f2a-11eb-8a74-9de1248b6b14.png)

---

generating random H-polytope using `gen_rand_hpoly()` and computing volume of hpolytop using `volume()` function of volesti.
### code
```R

# generate a random 10-dimensional polytope with 50 facets
randomHPolytop = gen_rand_hpoly(10, 53)

volume(randomHPolytop)
```
### output
```
1.263099e+17
```

---

## medium-test

### Objective : 
To run a test for integral of `f(x) = exp^{-a||x||^2}` over the cube `[-1,1]^n`, for various values of a and dimension `n` until it crashes.


### code 
i have used `cubintegrate()` function of `cubature` for finding integration of f(X) over an n-dimensional cube(hypercube).
```R
library('cubature')

d = 1

while(T){
  for (a  in c(-3:3)){

    eqn = function(x){ 
      norm_value = 0
      for (xi in x){
          norm_value = norm_value + xi^2
        }
        exp(-a*norm_value) 
    }

    integral <- cubintegrate(f = eqn, lower = rep(-1,d) , upper =  rep(1,d))
    print(paste('[ a =', a, ']', '    [ d =', d, ']     ', integral$integral ))
  }
    d <- d+1
}
```
i have taken value of `a` from `-3` to `3`. where as dimensions value `d` starts from 1 to  increment till program crashes ( when `d >= 24` ).
```sh
[1] "[ a = -3 ]     [ d = 1 ]      8.44442398577702"
[1] "[ a = -2 ]     [ d = 1 ]      4.72890778561042"
[1] "[ a = -1 ]     [ d = 1 ]      2.92530349181436"
[1] "[ a = 0 ]     [ d = 1 ]      2"
[1] "[ a = 1 ]     [ d = 1 ]      1.49364826562485"
[1] "[ a = 2 ]     [ d = 1 ]      1.19628801332263"
[1] "[ a = 3 ]     [ d = 1 ]      1.00868712046288"
[1] "[ a = -3 ]     [ d = 2 ]      71.3082963730533"
[1] "[ a = -2 ]     [ d = 2 ]      22.362568799364"
[1] "[ a = -1 ]     [ d = 2 ]      8.55740049207686"
[1] "[ a = 0 ]     [ d = 2 ]      4"
...
[1] "[ a = 0 ]     [ d = 23 ]      8388608.00000001"
[1] "[ a = 1 ]     [ d = 23 ]      12368647.0617252"
[1] "[ a = 2 ]     [ d = 23 ]      29259348.6431253"
[1] "[ a = 3 ]     [ d = 23 ]      36107139.7942574"
[1] "[ a = -3 ]     [ d = 24 ]      0"
[1] "[ a = -2 ]     [ d = 24 ]      0"
...
 *** caught segfault ***
address 0x7fa120125050, cause 'invalid permissions'
malloc(): corrupted top size
Aborted (core dumped)
```
[complete output for medium test ](https://github.com/sutharp777/test-for-geomScale/issues/1)

--- 

## hard-test

### Objective
Use volesti to approximate the same integrals as in the previous test by **simple Monte Carlo** based on **uniform sampling** and by Importance Sampling using multivariate spherical Gaussian. 
Comment on the accuracy and run-time.

---

### Observation for `f(x) = exp^{-a||x||^2}`

cubature | volesti
----------|--------
using the *`cubature`* r package we can compute integration up to 23-dimensional functions. | using *`volesti`* approximation we can compute up to a few hundreds of dimensions.
`cubintegrate()` uses Monte-Carlo multidimensional sampling over hypercubes for integration [[1]](https://bnaras.github.io/cubature/reference/index.html) | uses Monte-Carlo Simulation with a uniform random walk hit-ran for uniform sampling over hypercubes, to approximate the volume efficiently in poly-time O(d^4).
cubature is more accurate for small dimensions d<=7 [[2]](https://github.com/stevengj/cubature), it breaks for higher dimension (d>=24) | whereas volesti is working fine even up to a hundred dimensions with an error around the error of ~2% or lesser, even it is Hit-and-run random walks.

### output using cubature
```sh
pradeep@7:~/Desktop/GeomScale$ time Rscript cubature-int.R 
[1] "[ a = -3 ]     [ d = 22 ]      55239072207717195776"

real	0m10.909s
user 0m10.708s
sys	0m0.400s
```

### output using volesti
```sh
pradeep@7:~/Desktop/GeomScale$ time Rscript volesti-int.R 
[1] "[ a = -3 ]     [ d = 22 ]      7.3243602036296e+19"

real	0m0.568s
user	0m0.688s
sys	0m0.165s
```

### comparison of output:
**volesti output**     : 

7.3243602036296e+19  ~=         73243602036296000000

**cubature output** :       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;             55239072207717195776

we can see that our order of output is almost the same ( same number of digits ).

---
