# workNotes

## 2023-07-24

### jquery removeClass 失效

问题描述：

点击按钮后给元素新增一个class：'scroll'，然后再次点击的时候，希望移除该class，结果发现并未移除；</br>
原因是addClass和removeClass之间时间间隔太断了，几乎是addClass后瞬间再次执行removeClass导致了未执行；

### 新增CSS技巧：

#### CSS选择器选择前几个元素

```
例如选择前三个元素
&:nth-child(-n+3){
  margin-top: 0;
}
```

## 2023-03-01

### 文字两端对齐

类似于如下效果：
```
航   班  号：Mu3678
预计旅客人数：10
目 前 出 行：50
```

css:text-align-last:justify;

```
text-align: justify;
text-align-last: right;
-moz-text-align-last: right; } /* 针对 Firefox 的代码 */
```

### text-align-last:justify不支持ios

```
.item {
 text-align: justify;
 text-align-last: right;
 -moz-text-align-last: right; } /* 针对 Firefox 的代码 */
  
  &:after {
    content: ‘’;
    width: 100%;
    height: 0;
    display: inline-block;
    visibility: hidden;
  }
}

```


## 2023-02-28

### 手机号验证

```
checkPhone() {
				let r = /^[1]([3-9])[0-9]{9}$/;
				if (!r.test(this.phone)) {
					uni.showToast({
						icon: 'none',
						title: '请输入正确的手机号'
					})
					return false;
				}
				return true
			},
```

## 2023-02-27 uniapp开发页面注意点

### 一：交互按钮需要注意连续点击可能会引起的问题

描述：新增页面中点击确定进行接口请求，请求后返回列表页。解决办法：</br>

```
data(){
 return {
  status:'loading',
 }
},
methods:{
 addData(){
  if(this.status == 'loading'){
   this.status = 'finished';
   addData().then(res=>{
    if(res.code == 200) {
     this.status = 'loading';
    }
   }).catech(error => {
    this.status = 'loading'
   })	
  }
  
 }
}
```

### 二：uniapp页面返回渲染问题



### 三：

## 2023-02-17

### closest() 导致的无限循环BUG修改

问题描述：

通过closest()查找当前的元素最近的指定父元素，判断该父元素下所有checkBox是否为勾选状态（通过类名判断），如果为否则该父元素不为勾选状态，若为是则勾选当前父元素（通过类名进行控制）。</br>
实现代码如下：
```
associated:function ($item) {
  let $parent = $item.closest(".tree);
  if($parent.length == 0) {
    ...
    associated($parent)
  }
} 
```

在执行过程中发现该方法陷入了死循环，一番探查之后原来是$parent 与$item是同一对象 </br>

重点！！！！

##### <font color="red">closest()方法从当前元素，沿 DOM 树向上遍历，直到找到已应用选择器的一个匹配为止。该匹配过程包括自身</font>

解决办法：

##### <font color="red">使用``` $(item).parents(".tree-item:eq(0)") ``` 替代该方法</font>



## 2023-02-08

### IE字体文件引用

描述：IE不支持ttf文件。可将ttf转化为woff文件

```
@font-face {
    font-family: 'YouSheBiaoTiHei';
    src: url('YouSheBiaoTiHei.woff'),
         url('YouSheBiaoTiHei.eot?#iefix') format('embedded-opentype');
}
```

### IE浏览器100vh进行大屏页面响应式适配时出现外边距合并的情况

问题描述：</br>
近期在完成一次前端大屏静态页面的过程中，使用vh与vw以及flex布局进行响应式兼容。结果发现IE浏览器对flex布局兼容性很差，导致页面底部多出一部分。

解决办法：</br>
```
body,html{
  width:100vw;
  height:100vh;
  overflow:hidden;//重点
}
```

### 扩展 - overflower:hidden的三个用法

#### 一：清除浮动

#### 二：溢出隐藏

#### 三：解决外边距塌陷

```
父级元素内部有子元素，如果给子元素添加margin-top样式，那么父级元素也会跟着下来，造成外边距塌陷，如下：
/*css样式*/
<style type="text/css">
    .box{ background:skyblue;}
    .kid{ width: 100px;height: 100px; background: yellow; margin-top: 20px}
</style>
 
/*html*/
<body>
    <div class="box">
	<div class="kid">子元素1</div>
    </div>
</body>

因此给父级元素添加overflower:hidden解决该问题
/*css样式*/
<style type="text/css">
    .box{ background:skyblue;
          overflow: hidden; /*解决外边距塌陷*/   
        }
    .kid{ width: 100px;height: 100px; background: yellow; margin-top: 20px}
</style>
 
/*html*/
<body>
    <div class="box">
	<div class="kid">子元素1</div>
    </div>
</body>
```


## 2023-01-12

### Vue 修改对象数组中新增属性的值时，页面未同步更新

问题描述：</br>
点击按钮切换播放与暂停状态，状态显示方式为图片，切换开关为新增属性。修改属性后，页面样式未同步更新到；</br>
解决办法：</br>

```
将 targetClass.isPlay = true 改为

this.$set(targetClass, "isPlay", true)
```

### uniapp页面滚动时自动切换Tab

思路：</br>
页面加载时获取目标元素距离页面顶部；</br>
```
//uniapp页面滚动监听方法
onPageScroll(e) {
  this.getTop();
},

onLoad(){
  this.getTop = this.debounce(function() {
	this.selectQuery('.desc').then(res => {
	  if (res.top < 0) {
	    this.tabActive = 1;
	  } else {
	    this.tabActive = 0;
	  }
	});
 }, 200);
}
```

```
	/**
	 * 获取dom元素样式
	 * @param {String} selector 选择器
	 */
	Vue.prototype.selectQuery = function(selector) {
		const selectQuery = uni.createSelectorQuery();
		return new Promise(resolve => {
			selectQuery.in(this).select(selector).boundingClientRect(res => {
				resolve(res);
			}).exec();
		})
	}
	
	// 使用时
	this.selectQuery('.componyIntro').then(res => {
		this.scrollTop[1] = res.top;
	});
```

## 2023-01-06

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















