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

