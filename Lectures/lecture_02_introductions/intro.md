# Introduction
## Example

mp: material proprety
* EX—Elastic moduli (also EY, EZ)
* PRXY—Major Poisson's ratios (also PRYZ, PRXZ)
* NUXY—Minor Poisson's ratios (also NUYZ, NUXZ)

et: define a local element "1" whose type is plane42

blc4: Creates a rectangular area or block volume by corner points. XCORNER, YCORNER, WIDTH, HEIGHT, DEPTH
## codes
```
/prep7  
mp,ex,1,30e6
mp,nuxy,1,0.3
et,1,plane42
blc4,0,0,10,1 ! XCORNER: 0, YCORNER: 0, WIDTH: 10, HEIGHT: 1, DEPTH: null

amesh,all
nsel,s,loc,x,0
d,all,ux
alls
d,node(0,0.5,0),uy 
f,node(10,1,0),fy,-100 
/solu 
solve 
/post1 
plns,u,y
```
