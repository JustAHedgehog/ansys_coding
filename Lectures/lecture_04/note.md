# 題目

![image](https://hackmd.io/_uploads/Sk6X6-H3Jl.png)

# 程式碼
```
FINISH
/CLEAR 

/FILNAME,ex5-1a,0   
/TITLE,ex5-1a. plane truss   

/PREP7  
  
ET,1,LINK1  
R,1,4, , 
 
MP, EX, 1, 29E6

!MPTEMP,1,0  
!MPDATA,EX,1,,29E6 

N,,0,0  
N,,6*12,0  
N,,12*12,0 
N,,3*12,4*12  
N,,9*12,4*12  

TYPE, 1   
MAT,  1
REAL, 1   

E,1,4   
E,1,2    
E,4,2    
E,2,3 
E,2,5 
E,5,3  
E,4,5

FINISH  

/SOLU  
ANTYPE,STATIC

D,1, ,0, , , ,UX,UY, , , , 
!D,1,UX,0
!D,1,UY,0  
D,3, ,0, , , ,UY, , , , ,
!D,3,UY,0
F,2,FY,-10000

SOLVE
FINISH

/POST1
PLDISP,1

PRRSOL,FX                   
PRRSOL,FY   

ETABLE,FORCE,SMISC, 1        
ETABLE,STRESS,LS, 1 
PRETAB,FORCE,STRESS 
```

# 各部分說明
## 前處理
```
ET,1,LINK1  
R,1,4, , 
MP, EX, 1, 29E6
```
1. 設定元素 LINK1。
2. 設定 LINK1 對應的常數 R1，對應他的截面積$A = 4 in$。
3. 設定彈性模數 EX1($E=29*10^6 ksi$)
:::info
:bulb:備註：現在ANSYS在Structral Mass項內只剩下LINK180以及LINK11，但因為要設定參數過多，所以建議用指令輸入LINK1
![image](https://hackmd.io/_uploads/B1W2JfS3ke.png)

:::

```
N,,0,0  
N,,6*12,0  
N,,12*12,0 
N,,3*12,4*12  
N,,9*12,4*12 

TYPE, 1   
MAT,  1
REAL, 1 

E,1,4   
E,1,2    
E,4,2    
E,2,3 
E,2,5 
E,5,3  
E,4,5
```
1. 設定節點N1~N5
2. 指定元素類型(TYPE)、材質(MAT)、常數(REAL)
![image](https://hackmd.io/_uploads/ry027GrhJx.png)
3. 各節點連線


## 施力狀況&邊界條件

```
D,1, ,0, , , ,UX,UY, , , , 
!D,1,UX,0
!D,1,UY,0  
D,3, ,0, , , ,UY, , , , ,
!D,3,UY,0
F,2,FY,-10000
```


![image](https://hackmd.io/_uploads/ryEFrzB21e.png)

![image](https://hackmd.io/_uploads/H1ysSfHn1g.png)

![image](https://hackmd.io/_uploads/B1eedMShkg.png)


![image](https://hackmd.io/_uploads/By1awMHhJg.png)

![image](https://hackmd.io/_uploads/rk1svGB31g.png)




## 查詢結果
```
/POST1
PLDISP,1

PRRSOL,FX                   
PRRSOL,FY  
```

![image](https://hackmd.io/_uploads/Sk308fS21e.png)

![image](https://hackmd.io/_uploads/HkFkvGrhJe.png)


```
ETABLE,FORCE,SMISC, 1        
ETABLE,STRESS,LS, 1 
PRETAB,FORCE,STRESS 
```
![image](https://hackmd.io/_uploads/r1lNuzS31x.png)

