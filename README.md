# workNotes

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


