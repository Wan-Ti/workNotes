# workNotes

## 2023-01*06

### 函数防抖
```
/*
	 * 防抖
	 */
	Vue.prototype.debounce = function(callback, duration, isFirstExecution = false) {
		let debounceTimer = null;
		return function(...args) {
			let ctx = this;
			const delay = function() {
				debounceTimer = null;
				if (!isFirstExecution) callback.apply(ctx, args);
			};
			let executeNow = isFirstExecution && !debounceTimer;
			clearTimeout(debounceTimer);
			debounceTimer = setTimeout(delay, duration);
			if (executeNow) callback.apply(ctx, args);
		};
	}	
```

## 2023-01-04

### 误将node_modules文件夹提交到远程仓库处理办法

```
git rm -r --cached node_modules   // 删除远程仓库中的文件夹
git commit -m "remove node_modules" //提交信息
git push //提交到远程仓库
```

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

## 2022-08-29

### 溢出隐藏与flex布局冲突

需求子元素溢出隐藏，因为父元素使用了flex布局，子元素无法正常隐藏。解决办法：</br>
子元素使用：``` flex：none ```

###  animate方法

animate用于实现动画效果。有四个参数：</br>
1：类型：对象，内容：最终实现效果；
2：动画持续时间；
3：运动形式：有两种，'linear'(匀速)和'swing'(慢快慢)
4：回调函数，动画结束后执行方法
```
$(".scroll").animate(
{
opacity:1,
width:400px,
height:200px
},
1000,
'linear',
{
   $(this).text("移除“）
})

```

## 2022-09-07

### Echart 文字自适应方法：

```
/* Echart文字自适应 */
function fontSize(res){
    let clientWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
    if (!clientWidth) return;
    let fontSize = clientWidth / 1920;
    return res * fontSize;
};
```

## 2022-10-27

### Vue实现电视机飘窗效果

```
<div class="fixed" @mouseenter="stoptime" @mouseleave="startTime" :style="{ display: display,top:top,left:left }">
  <span class="close_img" @click="close()">关闭X</span>
</div>

data() {
  return {
      display: "block",
      top:'0px',
      left:'0px',
      timeInveral:null,
    };  
},
mounted(){
    this.fixedImg();
},
destoryed(){
    clearInterval(this.timeInveral);
},  
methods: {
  close() {
      this.display = "none";
    },
    fixedImg() {
      let iw = 150;//图片的宽度
      let ih = 200;//图片的高度
      let ws = parseInt(this.left);//当前图片与浏览器页面左边框距离
      let hs = parseInt(this.top);//当前图片与浏览器页面上边框距离
      let screenw = window.innerWidth;//浏览器页面宽度
      let screenh = window.innerHeight;//浏览器高度
      let cw = screenw - iw;//图片left属性最大值
      let ch = screenh - ih;//图片top属性最大值
      let wss = 1;//每次水平移动距离
      let hss = 1;//每次垂直移动距离
      //启动定时器
      this.timeInveral = setInterval(() => {
        ws += wss; hs += hss;
        if (hs > ch) {
          hs = ch; hss = -hss;
        }
        if (hs <= 0) {
          hss = -hss;
        }
        if (ws > cw) {
          ws = cw; wss = -wss;
        }
        if (ws <= 0) {
          wss = -wss;
        }
        this.top = hs + "px";
        this.left = ws + "px";
      }, 15);
    },
    stoptime(){
      clearInterval(this.timeInveral);
    },
    startTime(){
      this.fixedImg();
    }  
},

备注：类名为fixed的元素需要设置定位为position:fixed;并调整相应的层级
```

### 















