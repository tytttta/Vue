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

**Vue.js的指令是以v-开头的，它们作用于HTML元素，指令提供了一些特殊的特性，将指令绑定在元素上时，指令会为绑定的目标元素添加一些特殊的行为，我们可以将指令看作特殊的HTML特性（attribute）。**


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

### 2.3.2 v-if指令：条件渲染指令，根据表达式的真假来删除和插入元素，它的基本语法如下：
````
v-if="expression"
````
expression是一个返回bool值的表达式，表达式可以是一个bool属性，也可以是一个返回bool的运算式。
实例：
````
<body>
		<div id="app">
			<h1>Hello, Vue.js!</h1>
			<h1 v-if="yes">Yes!</h1>
			<h1 v-if="no">No!</h1>
			<h1 v-if="age >= 25">Age: {{ age }}</h1>
			<h1 v-if="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		
		var vm = new Vue({
			el: '#app',
			data: {
				yes: true,
				no: false,
				age: 28,
				name: 'keepfool'
			}
		})
	</script>
````
![v-if](https://github.com/tytttta/Vue/blob/master/vue_if.gif)

**注意：v-if指令是根据条件表达式的值来执行元素的插入或者删除行为。**

这一点可以从渲染的HTML源代码看出来，面上只渲染了3个\<h1\>元素, v-if值为false的\<h1\>元素没有渲染到HTML。


### 2.3.3 v-show指令
v-show也是条件渲染指令，和v-if指令不同的是，使用v-show指令的元素始终会被渲染到HTML，它只是简单地为元素设置CSS的style属性。

````
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1>Hello, Vue.js!</h1>
			<h1 v-show="yes">Yes!</h1>
			<h1 v-show="no">No!</h1>
			<h1 v-show="age >= 25">Age: {{ age }}</h1>
			<h1 v-show="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		
		var vm = new Vue({
			el: '#app',
			data: {
				yes: true,
				no: false,
				age: 28,
				name: 'keepfool'
			}
		})
	</script>
</html>
````
![v-show](https://github.com/tytttta/Vue/blob/master/vue_show.png)

### 2.3.4 v-else指令
可以用v-else指令为v-if添加一个“else块”。v-else元素必须立即跟在v-if或v-else-if元素的后面——否则它不能被识别.
**vue2 v-else 不支支持v-show**
````
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1 v-if="age >= 25">Age: {{ age }}</h1>
			<h1 v-else>Name: {{ name }}</h1>
			<h1>---------------------分割线---------------------</h1>
			<h1 v-show="name.indexOf('keep') >= 0">Name: {{ name }}</h1>
			<h1 v-else>Sex: {{ sex }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				age: 28,
				name: 'keepfool',
				sex: 'Male'
			}
		})
	</script>
</html>
````

### 2.3.5 v-for指令
v-for指令基于一个数组渲染一个列表，它和JavaScript的遍历语法相似：
````
v-for="item in items"
````
items是一个数组，item是当前被遍历的数组元素。
````
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<link rel="stylesheet" href="styles/demo.css" />
	</head>

	<body>
		<div id="app">
			<table>
				<thead>
					<tr>
						<th>Name</th>
						<th>Age</th>
						<th>Sex</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="person in people">
						<td>{{ person.name  }}</td>
						<td>{{ person.age  }}</td>
						<td>{{ person.sex  }}</td>
					</tr>
				</tbody>
			</table>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				people: [{
					name: 'Jack',
					age: 30,
					sex: 'Male'
				}, {
					name: 'Bill',
					age: 26,
					sex: 'Male'
				}, {
					name: 'Tracy',
					age: 22,
					sex: 'Female'
				}, {
					name: 'Chris',
					age: 36,
					sex: 'Male'
				}]
			}
		})
	</script>

</html>
````

### 2.3.6 v-bind指令
v-bind指令可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性（attribute），例如：v-bind:class
````
v-bind:argument="expression"
````
下面这段代码构建了一个简单的分页条，v-bind指令作用于元素的class特性上。
这个指令包含一个表达式，表达式的含义是：高亮当前页。
````
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<link rel="stylesheet" href="styles/demo.css" />
	</head>
	<body>
		<div id="app">
			<ul class="pagination">
				<li v-for="n in pageCount">
					<a href="javascripit:void(0)" v-bind:class="activeNumber === n? 'active' : ''">{{ n }}</a>
				</li>
			</ul>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				activeNumber: 1,
				pageCount: 10
			}
		})
	</script>
</html>
````
![v-bind](https://github.com/tytttta/Vue/blob/master/vue_bind.png)

**注意v-for="n in pageCount"这行代码，pageCount是一个整数，遍历时n从1(Vue2.0)开始，然后遍历到pageCount结束**

### 2.3.7 v-on指令
v-on指令用于给监听DOM事件，它的用语法和v-bind是类似的，例如监听\<a\>元素的点击事件：
````
<a v-on:click="doSomething">
````
有两种形式调用方法: **绑定一个方法(让事件指向方法的引用),或者使用内联语句。**
````
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<p><input type="text" v-model="message"></p>
			<p>
				<!--click事件直接绑定一个方法-->
				<button v-on:click="greet">Greet</button>
			</p>
			<p>
				<!--click事件使用内联语句-->
				<button v-on:click="say('Hi')">Hi</button>
			</p>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				message: 'Hello, Vue.js!'
			},
			// 在 `methods` 对象中定义方法
			methods: {
				greet: function() {
					// // 方法内 `this` 指向 vm
					alert(this.message)
				},
				say: function(msg) {
					alert(msg)
				}
			}
		})
	</script>
</html>
````

### 2.3.8 v-bind和v-on的缩写
Vue.js为最常用的两个指令v-bind和v-on提供了缩写方式。v-bind指令可以缩写为一个冒号，v-on指令可以缩写为@符号。
````
<!--完整语法-->
<a href="javascripit:void(0)" v-bind:class="activeNumber === n  ? 'active' : ''">{{n}}</a>
<!--缩写语法-->
<a href="javascripit:void(0)" :class="activeNumber=== n ? 'active' : ''">{{n}}</a>

<!--完整语法-->
<button v-on:click="greet">Greet</button>
<!--缩写语法-->
<button @click="greet">Greet</button>
````




