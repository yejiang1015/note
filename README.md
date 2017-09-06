## 随手记笔记

##### 	最初的念想

##### 	xie_bokeyuan45

### 隐藏滚动条

```` 
.content {
	-ms-overflow-style:none;
	overflow:-moz-scrollbars-none;
}
.content::-webkit-scrollbar{
	width:0px
}
/*
	设置滚动条的宽度问0
	火狐浏览器-moz-测试之后不兼容
*/
````

### 在页面中禁止使用右键

##### jQuery事件方法

````
$("body").on("contextmenu selectstart copy touchmove",function(){
	return false;
});
/*
	禁止右键菜单。复制等
*/
````

##### 行内代码方法——写在要禁止的标签上

````
ondragstart="window.event.returnValue=false" // 配合下面两个使用
onselectstart="event.returnValue=false"  // 禁止用户选择
oncontextmenu="window.event.returnValue=false" // 禁止用户右键
````

### angular阻止事件冒泡和阻止事件默认行为

````
e.stopPropagation();
e.preventDefault();
````

### 格式化时间

````
var getTime = function(num) {
	/*num = 0 为当前时间 为负数为前天之前 为整数位未来的天*/
    var dd = new Date();
     dd.setDate(dd.getDate() + num);
     var y = dd.getFullYear(), m = dd.getMonth() + 1, d = dd.getDate();
     var ss = dd.getHours(), ff = dd.getMinutes(), mm = dd.getSeconds();
     if (m >= 1 && m <= 9) {
        m = "0" + m;
     }
     if (d >= 0 && d <= 9) {
        d = "0" + d;
     }
     if (ss >= 0 && ss <= 9) {
        ss = "0" + ss;
     }
     if (ff >= 0 && ff <= 9) {
        ff = "0" + ff;
     }
     if (mm >= 0 && mm <= 9) {
        mm = "0" + mm;
     }
     var nowtime = y + "-" + m + "-" + d + ' ' + ss + ':' + ff + ':' + mm;
     return nowtime;
};
console.log(getTime(-1))//获取上一天的时间
````

### chrome浏览器记住密码会改变表单样式

##### 	form标签上添加 autocomplete='off'   可禁止chrome 浏览器自动填充表单  简称记住密码

### 改变表格计算方法（渲染方法）

##### 	表格标签的td设置了宽度。但是内容太多会被挤大了。设置的宽度就没有作用了。要实现表格td溢出隐藏

````
/*表格td内文本溢出隐藏*/
table.tableLayout {
	table-layout:fixed;
}
table.tableLayout  th.tdLayout,
table.tableLayout  td.tdLayout {
	white-space:nowrap;
    text-overflow:ellipsis;
    -o-text-overflow:ellipsis;
    overflow: hidden;
}
/*
	使用时在table标签上加上tableLayout类名在td标签上加上tdLayout类名在设置td的宽度即可实现td文字溢		出...的效果
*/
````

### 加载更多

##### 所对应的是body滚动（应该，写的时候忘记了没有去看）

````
export const loadMore = (element, callback) => {
	let windowHeight = window.screen.height;
	let height;
	let setTop;
	let paddingBottom;
	let marginBottom;
    let requestFram;
    let oldScrollTop;

    document.body.addEventListener('scroll',() => {
       loadMore();
    }, false)
    //运动开始时获取元素 高度 和 offseTop, pading, margin
	element.addEventListener('touchstart',() => {
        height = element.offsetHeight;
        setTop = element.offsetTop;
        paddingBottom = getStyle(element,'paddingBottom');
        marginBottom = getStyle(element,'marginBottom');
    },{passive: true})

    //运动过程中保持监听 scrollTop 的值判断是否到达底部
    element.addEventListener('touchmove',() => {
       loadMore();
    },{passive: true})

    //运动结束时判断是否有惯性运动，惯性运动结束判断是非到达底部
    element.addEventListener('touchend',() => {
       	oldScrollTop = document.body.scrollTop;
       	moveEnd();
    },{passive: true})
    
    const moveEnd = () => {
        requestFram = requestAnimationFrame(() => {
            if (document.body.scrollTop != oldScrollTop) {
                oldScrollTop = document.body.scrollTop;
                loadMore();
                moveEnd();
            }else{
            	cancelAnimationFrame(requestFram);
            	//为了防止鼠标抬起时已经渲染好数据从而导致重获取数据，应该重新获取dom高度
            	height = element.offsetHeight;
                loadMore();
            }
        })
    }

    const loadMore = () => {
        if (document.body.scrollTop + windowHeight >= height + setTop + paddingBottom + marginBottom) {
            callback();
        }
    }
}
````





### Bootrtap

##### Bootrtap使用场景：移动端和PC端一体式开发且PC最大宽度为1200以下。不适合1920px版心大屏响应式开发方式。目前可用方式为流式布局百分比布局



### vuejs填坑-----------veex <--->react-native

##### 基于vue-cli脚手架（webpack）

引入依赖模块方法：require();

导出依赖模块方法：export default {}   //导出一个对象

##### 单文件组件.vue

引入单文件组件模块： import `name` from `'url'`

js中  export default {}

css引入依赖样式文件 @import '`url`' 

##### vue-cli初始化项目

1. vue init webpack-simple `appname`
2. vue init webpack `appname`

###### 二者的区别。目前为止编译打包的时候第一个不打包index.html只是打包组件和资源--------------- 而第二种方法连同index一起打包为一个新的文件-----------两种方法中首选第二种方法（没有原因）

##### 在初始化时（注意）

##### 在初始化时需要写入一些简单的默认配置。其中一项ESLint   提示为是否开启这个功能。在项目中选择no。注意不要一直回车。那样到后面写代码会有很多的报错。那是代码风格检查。很不容易遵守。项目中没有必要检查。如果已经安装上这个插件了需要把它删除。可以

​	`打开webpack.base.cofig文件把和eslint相关的代码都删除了重新启动服务即可`

### 启动项目

````
npm run dev
npm run build
````



### css布局盒子模型

###### 前提屏幕全屏无滚动条、响应式变换兼容性IE8头部的高度可设置百分比。相应的内容盒子的top值也是设置百分比![img](file:///C:\Users\Administrator\AppData\Roaming\Tencent\Users\1091470584\QQ\WinTemp\RichOle\2{P5PVH6~0J2[S}NQ`THNMU.png)

````
.box {
  position: relative;
}
.boxchildtop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100px;
}
.boxchildmain {
  position: absolute;
  top: 100px;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
}
````

### js闭包

##### js闭包不是简单的return一个数据回来，而是在函数中嵌套一个函数，并返回这个函数。然后在闭包函数中得到要获取的内容。就形成了闭包、在使用的时候首先调用最外面的函数得到一个返回值。返回值是一个函数也就是闭包函数的载体。将其赋值给一个变量。这个变量就被改变为一个函数。因为闭包的载体是一个函数。当要使用这个数据的时候就调用闭包载体的这个变量函数。就能得到这个闭包回来的数据

````
	function f1(){
　　　　n=999;
　　　　return function f2(){
			return n;
　　　　}
　　}
　　var result=f1();
	var bibaon = result();
	console.log(bibaon)
	// 闭包。函数嵌套函数。两个函数，获取数据的时候调用两个函数
````

### css弹性盒子

一个元素设置了弹性盒子属性。可以控制内容的居中方式。

````
<div class"a">内容</div>
<style>
	.a {
      	display: flex;
      	justify-content: center;
        align-items: center;
	}
</style>
justify-content: center;//设置水平方向的布局。
align-items: center;//设置垂直方向的布局。兼容性IE 10 

// 在使用弹性盒子中。如果有一个固定宽高的和子。里面的内容可以滚动。并且内容高度大于盒子高度有滚动
// 那么如果设置了align-items: center; 盒子里的内容就会居中。这是在盒子里面滚动是看不到内容的头部的，只能看到内容的居中部位和底部
// 要想能再滚动中看到头部和底部的内容。就不能使用align-items:cener;这个属性；两者有冲突
````

#### Vue axios

````
https://github.com/mzabriskie/axios

npm install axios --save-dev

main.js----
import axios from 'axios';
Vue.prototype.axios = axios;

vue components-----
this.axios({
  method: 'get/post',
  url: 'url',
  params: {
    data1: '1',
    data2: '2'
  }
}).then(function(res){
  
}).catch((error)=>{
  
})
````

### vue-router多层嵌套书写规则

````
// 当页面有多层路由嵌套时。路由规则需要严格书写。不然容易出错误
路由文件：按照正常的路由规则书写。按照父子嵌套的规则。子路由名不用带父路由名字
跳转路径：就是router-link 的to属性的值。需要附带父路径的名字。
path: 'main',
      component: r => require(['./view/main.vue'], r),
      children: [
        {
          path: 'loan', // 农信贷
          component: r => require(['./view/loan/index.vue'], r),
          children: [
            { path: 'loanamount', component: r => require(['./view/loan/loanamount.vue'], r) }, // 借款
            { path: 'query', component: r => require(['./view/loan/query.vue'], r) }, // 借款
            { path: 'withdrawals', component: r => require(['./view/loan/withdrawals.vue'], r) }, // 借款
          ],
        },
        
        
{
    name: '农信贷',
    path: '/main/loan',
    icon: '',
    badege: '0',
  },
  {
    name: '养殖管理',
    path: '/main/manage',
    icon: '',
    badege: '0',
  },
  // 如果/main/manage下面还有子路由，那么书写就是
  /main/manage/子路由名称，以此类推
````



### git命令

````

git config配置本地仓库
常用git config --global user.name、git config --global user.email
git config --list查看配置详情
git init 初始一个仓库，添加--bare可以初始化一个共享（裸）仓库
git status 可以查看当前仓库的状态
git add“文件” 将工作区中的文件添加到暂存区中，其中file可是一个单独的文件，也可以是一个目录、“*”、-A
git commit -m '备注信息' 将暂存区的文件，提交到本地仓库
git log 可以查看本地仓库的提交历史
git branch查看分支
git branch“分支名称” 创建一个新的分支
git checkout“分支名称” 切换分支
git checkout -b deeveloper 创建并切到developer分支
git merge“分支名称” 合并分支
git branch -d “分支名称” 删除分支
git clone “仓库地址”获取已有仓库的副本
git push origin “本地分支名称:远程分支名称”将本地分支推送至远程仓库，
git push origin hotfix（通常的写法）相当于
git push origin hotfix:hotfix
git push origin hotfix:newfeature
本地仓库分支名称和远程仓库分支名称一样的情况下可以简写成一个，即git push “仓库地址” “分支名称”，如果远程仓库没有对应分支，将会自动创建
git remote add “主机名称” “远程仓库地址”添加远程主机，即给远程主机起个别名，方便使用
git remote 可以查看已添加的远程主机
git remote show “主机名称”可以查看远程主机的信息
git remote -v 查看远程
````

````
git remote 列出远程主机列表
git remote add origin gitpath 添加远程仓库
git push -u origin master 提交到远程仓库1
git push 提交到远程仓库2
git push -u 提交到远程仓库3
git pull 更新本地代码
````

