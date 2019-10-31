####简要说明
>使用vue脚手架创建的vue项目均为单页面应用。但是有时候我们也需要多页面应用，那该如何使用cli来配置工程呢？看了很多教程都写得太粗糙了，大致说清楚了，但是很多细节不到位，所以我决定自己写一篇文章来说明用法。
###Vue CLI3开发多页面应用
####第一步
按照以往的方式用CLI创建vue工程，此时的工程是一个单页面的vue工程。想必大家都很熟悉了。
```
vue create 项目名 或者 vue ui（创建项目图形化界面，有点意思）
```
可以看一下此时新建的单页面项目的目录结构：
我们会发现我们的项目里没有vue.config.js这个配置文件，需要手动增加。
![单页项目结构](https://upload-images.jianshu.io/upload_images/7887714-fd0525762ecb90ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####第二步
更改项目目录结构
1.新建pages文件夹（这个文件夹名字可以随意）。
2.在pages下再新建一层目录用来存放多页面的模板html和入口Js文件。（个人觉得这种目录结构会比较清晰，可以看下图）。
![目录结构](https://upload-images.jianshu.io/upload_images/7887714-dd75b44401e3e21a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####第三步
修改vue.config.js这个配置文件，因为是多页面，所以要配置pages。配置如下：
1.手动配置
**注意:**配置的filenam（也就是：about1，home）可以随意取名，但是下面设置初始入口的时候 会用到 。
```
module.exports = {
pages：{
     about1:{
		entry:"./src/pages/about1/index.js",
		template:"./src/pages/about1/index.html",
		filename:"index1.html",
		name:"昆明爱讴科技有限公司"
	},
	home:{
		entry:"./src/pages/home/index.js",
		template:"./src/pages/home/home.html",
		filename:"home1.html",
		name:"昆明爱讴科技有限公司"
	}
}
}
```
2.也可以借助glob这个库来查找入口模板和入口js文件，如下：
**注意:**项目目录结构不同，下面路径的截取也不一样，需要自己写方法。
```
//配置pages多页面获取当前文件夹下的html和js
function getEntry(globPath) {
	let entries = {};
	glob.sync(globPath).forEach(function(entry) {
		var tmp = entry.split('/').splice(-3);
		entries[tmp[1]] = {
			entry: 'src/' + tmp[0] + '/' + tmp[1] + '/' + 'index.js',
			template: 'src/' + tmp[0] + '/' + tmp[1] + '/' + 'index.html',
			filename: tmp[1]
		};
	});
	return entries;
}
```
####第四步
配置入口页面：index：上面定义的页面的filename(也就是：about1，home)
```
  devServer: {
    index:"home1.html",
    open: process.platform === "darwin",
    disableHostCheck: false,
    host: "0.0.0.0",
    port: 8088,
    https: false,
    hotOnly: false, // See https://github.com/vuejs/vue-cli/blob/dev/docs/cli-service.md#configuring-proxy
    proxy: null // string | Object
    // before: app => {}
  }
```
####第五步
```
运行 npm run serve  就可以看到你成功了
```


**项目地址demo:**
