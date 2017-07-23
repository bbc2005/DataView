# DataView

> 将数据通过Echarts图表的形式展现出来

> 单击图表的数据视图用layer弹出新页面

> 新页面用handsontable显示Excel样式电子表格

> 电子表格可以下载（只兼容Google最新浏览器）



# 技术栈

- echarts  3.6.2 展示图表
- handsontable 0.32.0 展示Excel样式电子表格
- layer-v3.0.3 Web弹层组件
- localStorage HTML5在客户端存储数据

# Echarts.js源码修改的地方

在3.6.2源码73097行：
```html
 if (typeof optionToContent === 'function') {
	var htmlOrDom = optionToContent(api.getOption());
	if (typeof htmlOrDom === 'string') {
		viewMain.innerHTML = htmlOrDom;
	}
	else if (zrUtil.isDom(htmlOrDom)) {
		viewMain.appendChild(htmlOrDom);
	}
}
```
改为如下：
```html
if (typeof optionToContent === 'function') {
   /* var htmlOrDom = optionToContent(api.getOption());
	if (typeof htmlOrDom === 'string') {
		viewMain.innerHTML = htmlOrDom;
	}
	else if (zrUtil.isDom(htmlOrDom)) {
		viewMain.appendChild(htmlOrDom);
	}*/
	var toGo='';
	toGo=optionToContent(api.getOption());
	//弹出即全屏
	var index = layer.open({
	  type: 2,
	  title: lang[0],
	  content: encodeURI(toGo),
	  area: ['100%' , '520px'],
	  anim :-1,
	  maxmin: true
	});
	layer.full(index);
	return ;
}
```

# Echarts实例option设置

在Echarts实例中设置option的toolbox如下：
```html
toolbox: {
	show: true,
	feature: {
	   /* dataZoom: {
			yAxisIndex: 'none'
		},*/
		dataView: {
			lang: ['预约日报表', '关闭', '下载'],
			readOnly: false,
			backgroundColor:'#39314a',
			textColor:'#fff',
			optionToContent: function(opt) {
				var otc='graph.html?formData=yuYueData&title=预约日报表';
				return otc;                
			}
		},
		magicType: {type: ['line', 'bar']},
		restore: {},
		saveAsImage: {}
	}
}
```
