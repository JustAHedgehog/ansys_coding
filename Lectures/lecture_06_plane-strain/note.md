# 04011圓管應變問題
## 題目
![image](https://hackmd.io/_uploads/ry3t1ZqgC.png)

## 程式碼
```
finish
/clear

/prep7
et,41,plane42, , ,2 !element type
mp,ex,24,210000 !matrial property
mp,prxy,24,0.3
!mp,ex,25,170000
!mp,prxy,25,0.5

k,1,0,0 !keypoint
k,2,200,0
k,3,500,0
k,4,0,200
k,5,0,500

LARC,4,2,1,200,
LARC,5,3,1,500,
L,4,5
L,2,3

AL,all

type,41
mat,24
real,
esys,0

esize, ,20 !20 per line

mshkey,1
amesh,1

DL,1, ,ALL, 
DL,3, ,SYMM !same ux,0
DL,4, ,UY,0 !same symm

SFL,2,PRES,1,1

/solu
solve
finish

/post1
rsys,1 !cylinder
!read result>by pick(S)
PLNSOL,S,X,0,1  
PLNSOL,S,Y,0,1  
!plot result>contour plot>nodal solu>stress>x/y
PATH,stress,2,30,60,
PPATH,1,0,200,0,0,0,
PPATH,2,0,500,0,0,0,

PDEF, ,S,X,AVG  
PDEF, ,S,Y,AVG  

PLPATH,SX,SY

PRPATH,SX,SY
```
## 各部分程式碼說明
因為圓管很長，可將應變視為0，因此將其簡化成平面應變問題。

![image](https://hackmd.io/_uploads/SyHs6OvTJe.png)

### 設定材質

```
/prep7
et,41,plane42, , ,2 !element type
mp,ex,24,210000 !matrial property
mp,prxy,24,0.3
```

老樣子，先設定基礎參數~~~~
元素就挑選PLANE42（命名為41），因為其可以計算平面應變（KETOPT(3)那邊），因此PLANE42的後方才會是, , ,2

![image](https://hackmd.io/_uploads/SkywWFwakx.png)

```
KEYOPT, 41, 3, 2
```

當然，你也可以特別寫一行設定KEYOPT，第一個輸入要針對的元素名稱，第二個針對要調整的KEYOPT，最後就是在KEYOPT(3)中要選哪一個(2)

然後再設定材料性質（$E=210 GPa$, $v =0.3$）

### 繪製模型

```
k,1,0,0 !keypoint
k,2,200,0
k,3,500,0
k,4,0,200
k,5,0,500
```

好的，因為他為對稱圖形接下來要來繪製以下的模型，首先創建關鍵點位，以後後續畫線
![image](https://hackmd.io/_uploads/ByAbBKD6kx.png)

```
LARC,2,4,1,200,  
LARC,3,5,1,500, 
```
![image](https://hackmd.io/_uploads/S1HpBFvpkx.png)

```
LSTR, 4, 5  
LSTR, 2, 3 
```

```
AL,1,4,2,3,
```
編號順序沒差
![image](https://hackmd.io/_uploads/ryaBYYDp1l.png)


### 指定材質

```
type,41
mat,24
real,
esys,0
```
因為沒有厚度，所以不用打Real Constant

### 切分網格
```
esize, ,20 !20 per line
```

```
LESIZE, ALL, , , 20
```
設定切分20等分
![image](https://hackmd.io/_uploads/Hk4ziKPaJg.png)

```
MSHKEY,1      ! mapped mesh 
AMESH,1 
```

### 施力

![image](https://hackmd.io/_uploads/SJQTitwTkl.png)

![image](https://hackmd.io/_uploads/ryGxntPpkl.png)

輸出結果如下，記得確認方向是否正確
![image](https://hackmd.io/_uploads/BJxBbntDTJe.png)

![image](https://hackmd.io/_uploads/BykHhYw6yl.png)

```
DL,1, ,ALL,0
```
固定線段L1，
![image](https://hackmd.io/_uploads/H1zIhKPaJe.png)
接著設定線段L4以及L3為對稱
![image](https://hackmd.io/_uploads/HkppnFwT1e.png)


輸出結果你會看到線段L4以及L3上有$S$的編號，代表設置成功，如下圖所示
![image](https://hackmd.io/_uploads/BJGZaFvTkg.png)


```
DL,3, ,UX,0 
DL,4, ,UY,0
```

當然你也可以針對每一條線段限制特定方向，但就需要思考鏡射鏡子該限制的移動方向，如L3的鏡射僅限於$Y$方向；L4的鏡射僅限於$X$方向。


### 查詢結果
```
RSYS, 1
```
如何轉成極座標
![image](https://hackmd.io/_uploads/rkDeg5vpkl.png)

如果是使用UI介面的話，則是按照下圖操作
![image](https://hackmd.io/_uploads/ryAfgcPpJg.png)

```
PLNSOL,S,X,0,1
PLNSOL,S,Y,0,1  
```
接著就可以查詢結果啦~~~~
X方向應力分布
![image](https://hackmd.io/_uploads/HJk1b5D61l.png)
Y方向應力分布
![image](https://hackmd.io/_uploads/HyLlZ9Pakl.png)


