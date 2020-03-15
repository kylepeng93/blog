---
title: jfreechart的使用心得
date: 2020-02-16 15:52:00
tags:
---
Jfreechart是一个开源的统计图表展示库，使用java代码编写，于各行各业中被广泛使用。
## 柱状图

### 调整柱状图的展示方位
``` java
	ChartFactory.createBarChart{
		"title",
		"X轴",
		"Y轴",
		PlotOrientation.HORIZONTAL, // 柱状图水平显示
		true,
		true,
		false);
```
其中 **PlotOrientation.HORIZONTAL** 控制柱状图水平展示，**PlotOrientation.VERTICAL** 控制柱状图垂直展示。

### 控制X轴的展示位置
``` java
	final CategoryPlot plot = chart.getCategoryPlot();
	plot.setRangeAxisLocation(AxisLocation.TOP_OR_LEFT);
```	
其中 **AxisLocation.TOP_OR_LEFT** 控制X轴的数据居于柱状图顶部展示。
另外，**AxisLocation.BOTTOM_OR_RIGHT** 控制X轴的数据居于柱状图底部展示。
