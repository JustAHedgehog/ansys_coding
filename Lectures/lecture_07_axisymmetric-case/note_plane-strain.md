# Axisymmetric Case

## 題目

![image](https://hackmd.io/_uploads/B1ML52xCJl.png)

## 程式碼

```
FINISH
/CLEAR

/FILNAME,ex5-6,0   
/TITLE,ex5-6. Thick-walled cylinder

/PREP7  

ET,1,PLANE42
KEYOPT,1,3,1        ! axisymmetric    

 
MP,EX,1,70000 
MP,NUXY,1,0.33  

BLC4,100,0,100,500  

ESIZE,10
TYPE,   1   
MAT,       1

MSHKEY,1
AMESH,1 

EPLOT

FINISH  

/SOLU
ANTYPE,STATIC

DK,1,UY,0                          ! Constrain motion in y
!DK,1, ,0, ,0,UY, , , , , ,        ! axial constraint
SFL,4,PRES,3,

SOLVE
FINISH

/POST1  

PLNSOL,S,X,0,1  
PLNSOL,S,Z,0,1  

PATH,stress,2,30,60,
PPATH,1,0,100,0,0,0,
PPATH,2,0,200,0,0,0,

PDEF, ,S,X,AVG  
PDEF, ,S,Z,AVG  
 
PLPATH,SX,SZ

PRPATH,SX,SZ

```

## 各部份說明

### 前處理
```
ET,1,PLANE42
KEYOPT,1,3,1  
! 也可以寫成 ET,1,PLANE42, 0, 0, 1
```

首先一樣設定元素性質PLANE42，並調整 $KEYOPT(3)=1$（軸對稱）。
:::info
如果寫成KEYOPT,1,3,1 
:::


```
MP,EX,1,70000 
MP,NUXY,1,0.33 
```
接著設定


```
BLC5,150,250,100,500
! BLC4,100,0,100,500 
```
BLC5是由中心點建立矩形；BLC4則是從邊邊建立
![image](https://hackmd.io/_uploads/HJ8An3eAJx.png)

#### 建立分割尺寸

```
ESIZE,10
```

![image](https://hackmd.io/_uploads/rklAphxCJx.png)


#### 指定材質

```
TYPE,   1   
MAT,       1
```

#### 開始切割

```
MSHKEY,1
AMESH,1 
```

![image](https://hackmd.io/_uploads/S1ObyagR1x.png)

### 設定邊界條件

#### 受力&位移
```
DK,1,UY,0                          ! Constrain motion in y
!DK,1, ,0, ,0,UY, , , , , ,        ! axial constraint
SFL,4,PRES,3,
```
因為軸對稱，且只有X方向受力，所以固定Y方向位移，X方向不固定。

接著查看施力線段為何(L4)，並在其上設定$3MPa$的壓力。
![image](https://hackmd.io/_uploads/ry5QG6lRJx.png)

### 檢視分析結果

#### 查看變形
```
PLDISP,1
```

首先可以查看變形結果，選擇"def+undeformed"，即可查看變形狀況
![image](https://hackmd.io/_uploads/S1SvmTgCye.png)

由左上角可知，變形最大量為0.009763m
![image](https://hackmd.io/_uploads/H1UQEpeA1l.png)

#### 應力分布
```
PLNSOL,S,X,0,1  
PLNSOL,S,Z,0,1
```

X方向($σ_{r}$)應力在這平面上的分布
![image](https://hackmd.io/_uploads/B1tHI6xRkg.png)

Z方向($σ_{θ}$)應力在這平面上的分布
![image](https://hackmd.io/_uploads/ByaQUTeA1x.png)

#### 繪製軸向應力分布線

```
PATH,stress,2,30,60,
PPATH,1,0,100,0,0,0,
PPATH,2,0,200,0,0,0,

PDEF, ,S,X,AVG  
PDEF, ,S,Z,AVG  
 
PLPATH,SX,SZ

PRPATH,SX,SZ
```

操作步驟請見上次課程內容。

去Preprocessor或者General Postproc內點擊Path Operations，接著透過By Nodes設定軸方向(X方向)的路徑。

![image](https://hackmd.io/_uploads/BkvRO6xR1e.png)

接著設定路徑名稱為stress

![image](https://hackmd.io/_uploads/SJvJYpeAkg.png)

確認後會產生這個表
![image](https://hackmd.io/_uploads/BJLzFTgRye.png)

:::info
注意：此介面千萬不可關閉，不然在做Map onto Path他會顯示找不到Path。
![image](https://hackmd.io/_uploads/S19JRpgCyg.png)
:::

```
PDEF, ,S,X,AVG  
PDEF, ,S,Z,AVG  
```

然後點擊Map onto Path以紀錄路徑數據，選擇Stress，並分別點選X方向以及Z方向，下方平均值記得勾選。
![image](https://hackmd.io/_uploads/H16qc6eCyg.png)

```
PLPATH,SX,SZ
```
最後將儲存的數據plot出來，記得分別點擊SX跟SZ再按OK。
![image](https://hackmd.io/_uploads/SJ-6hpg0yx.png)

輸出結果如下：
![image](https://hackmd.io/_uploads/HkQzUpxC1l.png)

```
PRPATH,SX,SZ
```
最後的最後，點擊List Path Items，並在視窗中同時點擊SX跟SZ就能輸出每一點的應力資訊了。
![image](https://hackmd.io/_uploads/HJn4yCeRJg.png)

輸出結果如下：

![image](https://hackmd.io/_uploads/rybtCaeRJe.png)
