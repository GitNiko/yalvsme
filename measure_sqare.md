测亩仪
==============================

## keyword
- 多边形面积计算
<!-- - 格林公式 -->
- 地图绘制线和面
- Mercator projection
- Albers projection
- Sinusoidal projection

## 方法
- 通过经纬度直接计算球面围成的多边形面积
- 通过经纬度转换成平面坐标后再计算多边形面积

## Mercator projection
非等面积，不是适合用于面积计算

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
[albers equal-area conic projection](http://mathworld.wolfram.com/AlbersEqual-AreaConicProjection.html)
[albers project in js](https://gist.github.com/RandomEtc/476238)
[mercator vs albers](https://gis.stackexchange.com/questions/49210/area-calculation-albers-equal-area-vs-pseudo-mercator)
[球面三角](https://www.guokr.com/article/98934/)
[正弦投影实现](https://www.periscopedata.com/blog/polygon-area-from-latitude-and-longitude-using-sql)
[正弦投影证明](http://www.progonos.com/furuti/MapProj/Normal/CartHow/HowSanson/howSanson.html)
[map projection](https://arxiv.org/pdf/1412.7690.pdf)