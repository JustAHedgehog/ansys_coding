# 題目
![image](https://hackmd.io/_uploads/SyjZvSAhkl.png)

# 程式碼
```
FINISH
/CLEAR

/FILNAME,ex5-4a,0   
/TITLE,ex5-4a. Plate with a hole

/PREP7   
ET,1,PLANE42
KEYOPT,1,2,0     ! extra displacement shapes
KEYOPT,1,3,3     ! plane stress with thickness input 

R,1,2,    ! thickness (mm)

!MPTEMP,1,0  
!MPDATA,EX,1,,210000 
!MPDATA,PRXY,1,,0.3  
MP,EX,1,210000
MP,PRXY,1,0.3
 
BLC4,0,0,150,50 
CYL4,0,0,20 
ASBA,1,2  

ASEL,S,AREA, ,3 
AATT,1,1,1,0,
ALLSEL,ALL
  
LESIZE, 5, , ,12, , , , ,1 
LESIZE,10, , ,12, , , , ,1 
LESIZE,2, , ,2, , , , ,1   
LESIZE,9, , ,12,0.2, , , ,1
LESIZE,3, , ,16,0.2, , , ,1

MSHKEY,0   ! free mesh 
AMESH,3 

FINISH  

/SOLU
ANTYPE,STATIC

SFL,2,PRES,-2,   
 
DL,10, ,UX,0  
!NSEL,S,LOC,X,0,0 
!DSYM,SYMM,X

!DL,10, ,SYMM  
  
DL,9, ,UY,0 
!NSEL,S,LOC,Y,0,0 
!DSYM,SYMM,Y

!DL,9, ,SYMM 

ALLSEL,ALL
SOLVE
FINISH

/POST1
PLNSOL,S,X,0,1  
PLNSOL,S,Y,0,1  
PLNSOL,S,EQV,0,1  
```
# 各部份說明
```
ET,1,PLANE42

KEYOPT,1,1,0
KEYOPT,1,2,0
KEYOPT,1,3,3
KEYOPT,1,5,0
KEYOPT,1,6,0
```
選用PLANE42
![image](https://hackmd.io/_uploads/r18jUBC2kl.png)

點選後結果如下，接著點擊options
![image](https://hackmd.io/_uploads/BJDRLBRnke.png)

進入並設定key options（如果該KEYOPT沒有要更改的話其實不用填上）
![image](https://hackmd.io/_uploads/HJYcDSC3kg.png)

```
R,324,2,
```

設定Real Constant，命名為324，再設定厚度為$2mm$
![image](https://hackmd.io/_uploads/ryFmurA3yx.png)

```
MP,EX,1,210000
MP,PRXY,1,0.3
```

進到Material Prpperty並設定楊氏模數$E$以及普松比$ν$
![image](https://hackmd.io/_uploads/H1H8tHA3Jl.png)

```
BLC4,0,0,150,50 
```

![image](https://hackmd.io/_uploads/r1CXTBR31l.png)

```
CYL4,0,0,20 
```

生成圓形，原點為$(0,0)$

![image](https://hackmd.io/_uploads/rysYaSAn1l.png)

```
ASBA,       1,       2  
```

選取要被切掉的地方（看左下角提示）

![image](https://hackmd.io/_uploads/SkxlCBC3yx.png)

選取要切掉的部分（看左下角提示）

![image](https://hackmd.io/_uploads/HJz-0r02Jg.png)

最終結果
![image](https://hackmd.io/_uploads/Sypz0HC31x.png)

```
TYPE,   1   
MAT,       1
REAL,     324   
ESYS,       0   
```

![image](https://hackmd.io/_uploads/Hy9p0H02Jg.png)

## 各線段切割


![image](https://hackmd.io/_uploads/H12KkI021x.png)

```
LESIZE, 5, , ,12, , , , ,1 
LESIZE,10, , ,12, , , , ,1 
LESIZE,2, , ,2, , , , ,1   
LESIZE,9, , ,12,0.2, , , ,1
LESIZE,3, , ,16,0.2, , , ,1
```
針對各線段進行分割，例：5號線段要切分成12份；10號線段要切分成12份；2號線段要切分成2份。
![image](https://hackmd.io/_uploads/HJf3gIRnyg.png)


```
MSHKEY,2
```
設定mesh key
![image](https://hackmd.io/_uploads/BJpNgI0n1g.png)

```
AMESH,ALL
```

最終輸出結果如下：
![image](https://hackmd.io/_uploads/BJ3ReIAnyx.png)

## 設定邊界條件

### 施力
```
SFL,2,PRES,-2,   
```
![image](https://hackmd.io/_uploads/B16cWI0n1x.png)

![image](https://hackmd.io/_uploads/BkDnbUR3yl.png)


### 設定對稱條件
```
DL,10, ,UX,0  
!NSEL,S,LOC,X,0,0 
!DSYM,SYMM,X

!DL,10, ,SYMM  
  
DL,9, ,UY,0 
!NSEL,S,LOC,Y,0,0 
!DSYM,SYMM,Y
```

![image](https://hackmd.io/_uploads/H1hrmL0nkx.png)

![image](https://hackmd.io/_uploads/rJst7U02kg.png)


```
ALLSEL,ALL
SOLVE
FINISH
```
## 輸出圖像

```
/POST1
PLNSOL,S,X,0,1  
PLNSOL,S,Y,0,1 
```
![image](https://hackmd.io/_uploads/ByVE4IA2yg.png)

```
PLNSOL, S,EQV, 0,1.0
```
切換應例分析圖為von Mises
![image](https://hackmd.io/_uploads/B1TRBLCn1x.png)
結果如下：
![image](https://hackmd.io/_uploads/BkibLIA3yg.png)



