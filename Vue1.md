# 一 开发准备

## 1.1 安装vscode
### 1.1.1 安装插件 view in browser 或 open in browser  

## 1.2 使用cdn引用vue
````
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
````
等

#  二 开发
## 2.1 MVVM模式
 MVVM模式 即 model-view-viewModel
 
 ![MVVM模型](https://github.com/tytttta/Vue/blob/master/mvvm.png)
 
 viewModel 是Vue的核心，它是一个Vue实例。Vue实例是作用于某一个HTML元素上的，这个元素可以是HTML的body元素，也可以是指定了id的某个元素。
 
 将上图中的DOM Listeners和Data Bindings看作两个工具，它们是实现双向绑定的关键。
从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据；
从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。

**实例**
````
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>

	<body>
		<!--这是我们的View-->
		<div id="app">
			{{ message }}
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		// 这是我们的Model
		var exampleData = {
			message: 'Hello World!'
		}

		// 创建一个 Vue 实例或 "ViewModel"
		// 它连接 View 与 Model
		new Vue({
			el: '#app',
			data: exampleData
		})
	</script>
</html>
````

使用Vue的过程就是定义MVVM各个组成部分的过程的过程。

1. 定义View
2. 定义Model
3. 创建一个Vue实例或"ViewModel"，它用于连接View和Model

在创建Vue实例时，需要传入一个 **选项对象**，选项对象可以包含数据、挂载元素、方法、模生命周期钩子等.

在这个示例中，选项对象的 **el属性** 指向View，**el: '#app'** 表示该Vue实例将挂载到\<div id="app"\>...\</div\>这个元素; **data属性** 指向Model， **data: exampleData**  表示Model是exampleData对象。

Vue.js有多种数据绑定的语法，最基础的形式是文本插值，使用一对大括号语法，在运行时 **{{ message }}** 会被数据对象的message属性替换，所以页面上会输出"Hello World!"。

## 2.3 指令
### 2.3.1 v-model指令：双向绑定

VM模式本身是实现了双向绑定的，在Vue.js中可以使用v-model指令在表单元素上创建双向数据绑定。
````
<!--这是我们的View-->
<div id="app">
	<p>{{ message }}</p>
	<input type="text" v-model="message"/>
</div>
````
将message绑定到文本框，当更改文本框的值时，**\<p\>{{ message }}\</p\>** 中的内容也会被更新。
![V-Model](https://github.com/tytttta/Vue/blob/master/v-model.gif)

反过来，如果改变message的值，文本框的值也会被更新，我们可以在Chrome控制台进行尝试。

> **Vue实例的data属性指向exampleData，它是一个引用类型，改变了exampleData对象的属性，同时也会影响Vue实例的data属性**









