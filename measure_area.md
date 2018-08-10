测亩仪
==============================
通过在地图上标记点，围成多边形后，给出该多边形区域的实际地里面积。

## keyword
- 多边形面积计算
<!-- - 格林公式 -->
- 地图绘制线和面
- equator(赤道), merdian(经线)
- Tissot’s indicatrix
- Mercator projection
- Albers projection
- Sinusoidal projection

## 方法
- 通过经纬度转换成平面坐标后再计算多边形面积(选用该方法)
- 通过经纬度直接计算球面围成的多边形面积

## 地图投影类型
`Carl Friedrich Gauss`得出一个球面无法在一地图上进行无损展示(a sphere’s surface cannot be represented on a map without distortion)。
所以根据损失类型来分类的话主要有以下三种类型:   
- Equal-area projections(等面积投影)
- Conformal projections(保角投影)
- Conventional projections(以上两种都有)

## Mercator projection
> 通过圆心投影到圆柱上后，平铺圆柱，形成平面坐标。特点是:非等面积，不是适合用于面积计算    

以下假设$\phi$和$\lambda$都是弧度单位,$\lambda = 0$的时候处在格林威治子午线, $\phi = 0$处在赤道 

在equator(赤道)处水平比例应该是 $s = w/(2\pi R)$  
在latitidue(纬度)为 $\phi$ 的位置时，水平比例: 

$$
s _ { h } = \frac { w } { 2 \pi R \cos \phi } = s \sec \phi 
$$

x轴:  

$$
x = \frac { w \lambda } { 2 \pi }
$$

y轴: 

$$
y = f ( \phi )
$$

特别的，当周长等于地图的宽度时($2\pi R = w$), $y = R\tan \phi$
在球面上$\phi$和$\phi_{1}$所对应的弧长差为:  

$$
R \left( \phi _ { 1 } - \phi \right)
$$

那么对应的地图上y的距离差就是$f(\phi_{1}) - f(\phi)$ 所以比例应该是:

$$
s _ { v } = \frac { 1 } { R } f ^ { \prime } ( \phi ) = \frac { 1 } { R } \lim _ { \phi _ { 1 } \rightarrow \phi } \frac { f \left( \phi _ { 1 } \right) - f ( \phi ) } { \phi _ { 1 } - \phi }
$$

`Mercator projection` 就是令水平比例等于垂直比例:  

$$
f ^ { \prime } ( \phi ) = \frac { w } { 2 \pi } \sec \phi
$$

根据积分表解出来:  

$$
y = f ( \phi ) = \frac { w } { 2 \pi } \ln | \sec \phi + \tan \phi |
$$


## Albers projection

### 特点
- 经纬线相互垂直
- 经线是直线
- 纬线是同心圆弧
- 等距，保形，等面积
- 极点表示为弧或点

## Sinusoidal projection

### 公式
$$
x = s R \Delta \lambda \cos \phi
$$
$$
y = s R \phi
$$
<!-- 
## 球面三角学
### 球面角 -->

## 多边形面积
### 公式
$$
S  = \frac { 1 } { 2 } \sum _ { k = 1 } ^ { m } \left( x _ { k } y _ { k + 1 } - x _ { k + 1 } y _ { k } \right)
$$

## 交互设计
- 长按标点，点之间连线围成区域。

## 流程
获取经纬度，通过`Albers projection`转换成平面坐标，计算面积

## 误差
实际上多边形边并不是直线，而是曲线



## 资料
[多边形面积公式推导](https://wenku.baidu.com/view/c6eb44c58bd63186bcebbc2a.html)  
[albers equal-area conic projection](http://mathworld.wolfram.com/AlbersEqual-AreaConicProjection.html)  
[albers project in js](https://gist.github.com/RandomEtc/476238)  
[mercator vs albers](https://gis.stackexchange.com/questions/49210/area-calculation-albers-equal-area-vs-pseudo-mercator)  
[球面三角](https://www.guokr.com/article/98934/)  
[球面多边形面积公式](http://mathworld.wolfram.com/SphericalPolygon.html)  
[正弦投影实现](https://www.periscopedata.com/blog/polygon-area-from-latitude-and-longitude-using-sql)  
[正弦投影证明](http://www.progonos.com/furuti/MapProj/Normal/CartHow/HowSanson/howSanson.html)  
[map projection](https://arxiv.org/pdf/1412.7690.pdf)  
[mapprojection内容更多](http://www.auburn.edu/academic/classes/fory/7470/lab08/understanding%20map%20projections.pdf)  