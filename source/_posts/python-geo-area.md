---
title: Python：根据经纬度计算距离及面积
date: 2020-03-16 15:33:52
tags: [python]
---

用到了两个库：`shapely`、`pyproj`，直接pip安装即可。

```python
from pyproj import Geod
from shapely.geometry import Point, LineString, Polygon

# 计算距离
line_string = LineString([Point(123.429056, 41.833526), Point(123.435235, 41.773191)])
geod = Geod(ellps="WGS84")
geod.geometry_length(line_string)

# 计算封闭区域面积、周长
geod = Geod(ellps="WGS84")
geod.geometry_area_perimeter(Polygon([
    (123.432746, 41.792136), (123.435922, 41.784584),
    (123.450857, 41.784392), (123.447681, 41.79252),
]))
```

<!--more-->

如果仅仅只是想要计算几何图形的距离及面积的话，可按下面方式计算：

```python
from shapely.geometry import Point, polygon

# 计算距离
Point(0,0).distance(Point(1,1))

# 计算封闭区域面积
polygon = Polygon([(0, 0), (1, 1), (1, 0)])
polygon.area

# 判断点是否在封闭区域内
polygon = Polygon([(0, 0), (2, 0), (2, 2), (0, 2)])
polygon.contains(Point(1, 1))       # True
polygon.contains(Point(3, 2))       # False
polygon.contains(Point(0, 2))       # False，在边界上

# 判断两个多边形是否重合
polygon = Polygon([(0, 0), (2, 0), (2, 2), (0, 2)])
polygon.contains(Polygon([(0, 0), (1, 1), (1, 0)]))     # True
polygon.contains(Polygon([(0, 0), (1, 1), (1, -1)]))    # False
```
