测亩仪
==============================


## keyword
- 多边形面积计算
- 格林公式
- 地图绘制线和面
- Mercator projection
- Albers projection

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



## 交互设计
- 长按标点，点之间连线围成区域。

## 流程
获取经纬度，通过`Albers projection`转换成平面坐标，计算面积


## 资料
[albers equal-area conic projection](http://mathworld.wolfram.com/AlbersEqual-AreaConicProjection.html)
[albers project in js](https://gist.github.com/RandomEtc/476238)
[mercator vs albers](https://gis.stackexchange.com/questions/49210/area-calculation-albers-equal-area-vs-pseudo-mercator)