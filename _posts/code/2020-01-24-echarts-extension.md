---
layout: blog
istop: true
comments: true
code: true
title:  "Echarts extension"
tags:
- echarts
- apache

background-image: https://www.echartsjs.com/zh/images/logo.png
date:   2020-01-24 13:42:00
category: code

---

Echarts Extension



Here are some unofficial extensions to Apache Echarts, implemented by [Yi Zhao](<https://scottyi.club/about?yi>).

1. Multiple charts joint zoom in/out and highlight:

   *Use action and event mechanism in Echarts, implement the joint zoom in/out and highlight function.*

   Trigger chart (Zoom in/out):

   **For highlight function, use mouse over and mouse out as  the event, use highlight as the action.**

```
// response to datazoom event
triggerChart.on('datazoom', function (evt) {
    doSomething();
    var option = triggerChart.getOption();
    var zoom = option.dataZoom[0];
    var start = zoom['start'];
    var end = zoom['end'];
    triggerChart.setOption(option, true);
    if (evt.event_status === true) {
        return;
    }
	targetChart.dispatchAction({
		// dispatch dataZoom 
        type: 'dataZoom',
        dataZoomIndex: 0,
        start: zoom.start,
        end: zoom.end,
        event_status: true
    });
}
```

​	Target Chart (Zoom in/out): 

```
targetChart.on('datazoom', function (evt) {
    doSomething();
    var option = targetChart.getOption();
    var zoom = option.dataZoom[0];
    var start = zoom['start'];
    var end = zoom['end'];
    targetChart.setOption(option, true);
    if (evt.event_status === true) {
        return;
    }
    triggerChart.dispatchAction({
        type: 'dataZoom',
        dataZoomIndex: 0,
        start: zoom.start,
        end: zoom.end,
        event_status: true
    });
}
```

1. Multiple granularity data zoom in/out: 

   Prepare x-axis list:

```
var xData1 = ['6-7', '7-8', '8-9', '9-10', '10-11', '11-12', '12-13', '13-14', '14-15', '15-16', '16-17', '17-18', '18-19', '19-20', '20-21', '21-22', '22-23', '23-24'];
var xData2 = ['6-6:30', '6:30-7', '7-7:30', '7:30-8', '8-8:30', '8:30-9', '9-9:30', '9:30-10', '10-10:30', '10:30-11', '11-11:30', '11:30-12', '12-12:30', '12:30-13', '13-13:30', '13:30-14', '14-14:30', '14:30-15', '15-15:30', '15:30-16', '16-16:30', '16:30-17', '17-17:30', '17:30-18', '18-18:30', '18:30-19', '19-19:30', '19:30-20', '20-20:30', '20:30-21', '21-21:30', '21:30-22', '22-22:30', '22:30-23', '23-23:30', '23:30-24'];
```

​	Prepare data list:

```
var option = {
    ...
    xdata: {
        seriesData1: [],
        seriesData2: []
    },
    ...
};
```

​	Use different data based on zoom level:

```
if (end - start < 50) {
    option['xAxis'][0].data = xData2;
    value = 2;
} else {
    option['xAxis'][0].data = xData1;
    value = 1;
}
option['series'] = option.xdata['seriesData' + value];
myChart.setOption(option, true);
```