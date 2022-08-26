# workNotes

## 前端样式规范：

### HTML + CSS

### js

1：功能代码包括变量要记得写注释，注释要简洁易懂；</br>
2:

## 2022/08/11

### H5端 逆地址解析-根据经纬度解析出相应地址

1：参考逆地址解析服务：https://lbs.qq.com/service/webService/webServiceGuide/webServiceGcoder 

2：uni.request出现跨域问题，改变manifest.json文件中关于H5配置

```
"devServer": {
	"https": false,
	"proxy": { //配置代理服务器来解决跨域问题，uniapp不适用CORS方案和设置JSONP方案
	"/locationApi": {
		"target": "https://apis.map.qq.com",
		"changeOrigin": true,
		"secure": false,
		"pathRewrite": {
			"^/locationApi": ""
		}
	  }
    }									
},
```

3：打包成H5后发现该方法只在开发环境生效，暂无解决方案；

## 2022/08/12 

### 解决clearInterval不生效的问题

情景复现：</br>
jsp项目中，左侧为菜单栏，右侧为动态数据生成的动态DOM，DOM渲染完成后，某个元素的长度大于一时开启定时器进行滚动效果展示，元素长度不大于一时不进行滚动。</br>
实际情况为长度大于一的元素正常滚动，但是菜单栏切换后，当前DOM中该元素长度为1却也在滚动，使用clearInterval也并不生效；

解决思路：</br>
通过了解setInterval发现，每个定时器生成的时候都有自己特定的ID，当有多个相同的定时器被调用的时候，使用clearInterval需要携带ID参数；

解决方案：</br>

```
let timer ;
let timeList = [];

timer = window.setImtervale(function () {
	console.log('haha')
},1000);
timeList.push(timer);

// 清除定时器时
tiemList.forEach((item,index) => {
	windwo.clearInterval(item);
})
tiemList = [];
timer = 0;
```

## 2022-08-26

### flex:1不要乱用

复现：</br>
之前做个一个页面，为左右布局，布局时左右宽度设置为flex:1;</br>
 
第一阶段：contentRight模块下的分模块需要添加滚动效果，但是子元素设置overflow:hidden失效；将子元素设置为85vh;

第二阶段：子元素固定宽度85vh,结果发现contentRight模块下的各个分模块不一样宽；检查元素发现vh是根据视口宽度进行计算的，因为不同高度的显示上会出现分模块宽度不一致；

解决办法：将父模块contentRight宽度从flex:1改为85vh;父模块下的子模块宽度设置为100%；



















