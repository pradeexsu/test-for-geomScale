## Test

  Easy: compile and run volesti. Read the CRAN package documentation, generate a random H-polytope and compute its volume.

## `volesti` project compiled and run Succesfully
#### build :
![build start](https://user-images.githubusercontent.com/49487927/112721557-78062880-8f2a-11eb-87f8-25d513bb4245.png)

![100 build](https://user-images.githubusercontent.com/49487927/112721554-763c6500-8f2a-11eb-9eff-05a7d6000edc.png)

#### test :
![testcase-passed](https://user-images.githubusercontent.com/49487927/112721558-79375580-8f2a-11eb-8a74-9de1248b6b14.png)

---

## generating random H-polytope using `gen_rand_hpoly()` function of volesti.

```R
library('volesti')

randomHPolytop = gen_rand_hpoly(13, 53)
#generate a 13-dimensional Hpolytope with 53 facets
randomHPolytop
```
### output:
```
An object of class "Hpolytope"
Slot "A":
             [,1]         [,2] ...
...
[53,] -0.034671511  0.1581493166 -0.168628193 -0.230467285

Slot "b":
 [1] 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10 10
[43] 10 10 10 10 10 10 10 10 10 10 10

Slot "volume":
[1] NaN

Slot "type":
[1] "Hpolytope"
```

### computing volume of hpolytop using `volume()` function of volesti
```R
volume(randomHPolytop)
```
### output
```
1.263099e+17
```

