# 簡支梁
本次教學透過操作UI介面完成範例
## 複習
beam3的結構
![image](https://hackmd.io/_uploads/SyeY0Tiokl.png)
beam188的結構
![image](https://hackmd.io/_uploads/B1ThC6sjyl.png)

## 題目
![image](https://hackmd.io/_uploads/HyAe1Asskg.png)

## 程式碼
```
FINISH
/CLEAR

/FILNAME,ex2a,0    
/TITLE, ex2a  cantilever beam.

/PREP7  

ET,1,BEAM188  
KEYOPT,1,4,2

MP, EX, 1, 12E9
MP, PRXY, 1, 0.3

!MPTEMP,1,0  
!MPDATA,EX,1,,12E9     
!MPDATA,PRXY,1,,0.3

SECTYPE, 1, BEAM, RECT, , 0   
SECOFFSET, CENT 
SECDATA,0.2,0.4,10,10,0,0,0,0,0,0  
SECPLOT, 1,1  

K,,0,0  
K,,3,0  
K,,4.5,0
K,,6,0  
 
K,,0,1

LSTR, 1, 2  
LSTR, 2, 3  
LSTR, 3, 4  

LPLOT
LATT,1, ,1, ,5, ,1

ESIZE,0.2,0,

LMESH,1,3,1 
NPLOT   
EPLOT

FINISH  


/SOLU
 
ANTYPE,STATIC

DK,1, ,0, ,0,ALL, , , , , ,  

LSEL,S, , , 1 
ESLL,S  
SFBEAM,ALL,1,PRES,2000, , , , , ,  
ALLSEL,ALL  

FK,3,FY,-4000   
FK,4,FY,-6000   

solve
finish

/post1

PLDISP,1

PLNSOL, U,Y, 2,1.0  
   
PLNSOL, U,X, 2,1.0  

/ESHAPE,1   

PLNSOL, S,X, 0,1.0  
```

## 各部分說明

### 設定keyopt
```
KEYOPT,1,4,2
```
設定beam188的性質(option)剪應力輸出K4為include both(torsional & transverse)
![image](https://hackmd.io/_uploads/Hk24gRii1e.png)
![image](https://hackmd.io/_uploads/HJnHl0sskg.png)

### 設定截面性質
```
!*  
/REPLOT,RESIZE  
SECTYPE,   1, BEAM, RECT, , 0   
SECOFFSET, CENT 
SECDATA,0.2,0.4,5,5,0,0,0,0,0,0,0,0 
!*  
SECPLOT,1,0 

```
點擊
![image](https://hackmd.io/_uploads/ByzsUAojkg.png)

![image](https://hackmd.io/_uploads/BJJnQRjoyg.png)

### 設定材料性質
```
!*  
MPTEMP,,,,,,,,  
MPTEMP,1,0  
MPDATA,EX,1,,12e9   
MPDATA,PRXY,1,,0
```
![image](https://hackmd.io/_uploads/SkvLGCioyl.png)
![image](https://hackmd.io/_uploads/rJZtMCioye.png)
![image](https://hackmd.io/_uploads/BJo2z0sj1e.png)

### 設定keypoints
```
K,1,0,0,0,  
K,2,3,0,0,  
K,3,4.5,0,0,
K,4,6,0,0,  
K,5,0,1,0,  
LSTR,       1,       2  
LSTR,       2,       3  
LSTR,       3,       4  

```
![image](https://hackmd.io/_uploads/SJiqu0jskl.png)

```
LATT,1, ,1, ,       5, ,1
```

```
ESIZE,0.2,0,
LMESH,all
```

### 施加分布力
先用box方式點選
![image](https://hackmd.io/_uploads/S1RPaRji1l.png)


![image](https://hackmd.io/_uploads/HyBbJknsJg.png)
![image](https://hackmd.io/_uploads/Byb41k3o1e.png)


```
/GO 
DK,P51X, ,0, ,0,ALL, , , , , ,  
```

![image](https://hackmd.io/_uploads/SkATkyho1x.png)

別忘了在之前先read results
![image](https://hackmd.io/_uploads/ByJYxy3i1x.png)


### 查詢x方向應力分布
```
PLNSOL, S,X, 0,1.0  
```