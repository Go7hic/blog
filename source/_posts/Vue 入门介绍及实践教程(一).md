---
layout: post
title: Vue 入门介绍及实践教程 (一)
date: 2015-09-25 21:00:04
tags: Vue
---

Vue.js 是国人 @尤小右 开发的一个轻量的 MVVM 库，在 Github 上有着 7k+ 的 start，其火热程度可见一斑，而且听说在岛国同行里很受欢迎。

在官网里，作者介绍说 Vue 专注 MVVM 模型的 ViewModel 层，通过双向数据绑定把 view 层和 Model 层连接起来。

![](http://cn.vuejs.org/images/mvvm.png)

在 Vue 里，ViewModel 是指 一个同步 Model 和 View 的对象，每个 Vue 实例都是一个 ViewModel。它们是通过构造函数 Vue 或其子类被创建出来的。比如：var vm = new Vue({})。
#### View 
View 则是指被 Vue 实例管理的 Dom 节点，vm.$el。每个 Vue 实例都关联着一个相应的 DOM 元素。当一个 Vue 实例被创建时，它会递归遍历根元素的所有子结点，同时完成必要的数据绑定。当这个视图被编译之后，它就会自动响应数据的变化。

#### Model 
Model 则是指一个轻微改动过的原生 JavaScript 对象，也可以称之为数据对象。一旦某对象被作为 Vue 实例中的数据，它就成为一个 “响应式” 的对象了。你可以操作它们的属性，同时正在观察它的 Vue 实例也会收到提示。
一旦数据被观察，Vue.js 就不会再侦测到新加入或删除的属性了。作为弥补，Vue 为被观察的对象增加 $add , $set和 $delete 方法。

下面是对 Vue.js 数据观测机制实现的高层概览：

![](http://cn.vuejs.org/images/data.png)

#### Directives 指令
在 Vue 里，指令表示带特殊前缀的 HTML 特性，可以让 Vue.js 对一个 DOM 元素做各种处理。比如：

```
<div v-text=“message”></div>
```

这里的 div 元素有一个 v-text 指令，其值为 message。Vue.js 会让该 div 的文本内容与 Vue 实例中的 message 属性值保持一致。
Directives 可以封装任何 DOM 操作。比如 v-attr 会操作一个元素的特性；v-repeat 会基于数组来复制一个元素；v-on 会绑定事件等。

Mustache 风格绑定，除了可以用指令来绑定外，还可以用mustache 风格的绑定，不管在文本中还是在属性中。它们在底层会被转换成v-text 和 v-attr 的指令。但是使用这个有两点需要注意的地方。

- 一个 `<image>` 的 src 属性在赋值时会产生一个 HTTP 请求，所以当模板在第一次被解析时会产生一个 404 请求。这种情况下更适合用 v-attr。
- Internet Explorer 在解析 HTML 时会移除非法的内联 style 属性，所以如果你想支持 IE，请在绑定内联 CSS 的时候始终使用 v-style。


#### Filters 过滤器 

过滤器是用于在更新视图之前处理原始值的函数。它们通过一个“管道”在指令或绑定中进行处理：

```
<div>{{message | capitalize}}</div>
```

这样在 div 的文本内容被更新之前，message 的值会先传给 capitalizie 函数处理

#### Components 组件
在 Vue.js，每个组件都是一个简单的 Vue 实例。一个树形嵌套的各种组件就代表了你的应用程序的各种接口。通过 Vue.extend 返回的自定义构造函数可以把这些组件实例化，不过更推荐的声明式的用法是通过 Vue.component(id, constructor) 注册这些组件。一旦组件被注册，它们就可以在 Vue 实例的模板中以自定义元素形式使用了。
#### 实践
了解了 Vue 的基本概念后，我们先来做个简单的例子：
1. `npm init` 初始化 package.json
2. `npm install vue vue-resources bootstrap --save -d` 安装需要的模块，vue 和 vue 插件以及 bootstrap 来写样式
3. touch index.html app.js 创建主页和 js 文件
4.index.html 代码：
```
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Vue Demo</title>
  <link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.min.css">
</head>
<body>

  <nav class="navbar navbar-default">
    <div class="container-fluid">
      <a class="navbar-brand"><i class="glyphicon glyphicon-bullhorn"></i> Vue Events Bulletin Board</a>
    </div>
  </nav>

  <div class="container" id="events">
    <!-- 添加内容 -->
    <div class="col-sm-6">
      <div class="panel panel-default">
        <div class="panel-heading">
          <h3>Add an Event</h3>
        </div>
        <div class="panel-body">
				<div class="form-group">
    				<input class="form-control" placeholder="title" v-model="event.name">
	 	 		</div>

		  		<div class="form-group">
	    			<textarea class="form-control" placeholder="内容" v-model="event.description"></textarea>
			  </div>

			  <div class="form-group">
	    			<input type="date" class="form-control" placeholder="日期" v-model="event.date">
			  </div>

			  <button class="btn btn-primary" v-on="click: addEvent">添加</button>
        </div>

      </div>
    </div>

    <!-- 显示添加的内容 -->
    <div class="col-sm-6">
      <div class="list-group">
      	<a href="#" class="list-group-item" v-repeat="event in events">
    			<h4 class="list-group-item-heading">
	      		<i class="glyphicon glyphicon-bullhorn"></i> 
      			{{ ev	ent.name }}
    			</h4>

    		<h5><i class="glyphicon glyphicon-calendar" v-if="event.date"></i> {{ event.date }}</h5>

    		<p class="list-group-item-text" v-if="event.description">{{ event.description }}</p>

    		<button class="btn btn-xs btn-danger" v-on="click: deleteEvent($index)">Delete</button>
  		</a>
      </div>
    </div>
  </div>

  <!-- JS -->
  <script src="node_modules/vue/dist/vue.js"></script>
  <script src="node_modules/vue-resource/dist/vue-resource.js"></script>
  <script src="app.js"></script>
</body>
</html>
```
5. app.js 代码
```
new Vue({
    el: '#events',// 管理的 DOM 节点
    data: {
        event: { name: '', description: '', date: '' },
        events: []
    },
    // 页面加载时会调用这个函数，执行里面的方法
    ready: function() {
        this.fetchEvents();
    },

    // 这里写你想要注册的方法
    methods: {
        fetchEvents: function() {
            var events = [];

            // model 里的 $set 方法，
            this.$set('events', events);
            // 如果你想从后台接口 get 获取 json 数据，可以这么写
            // this.$http.get('api/events').success(function(events) {
            //   this.$set('events', events);
            // }).error(function(error) {
            //   console.log(error);
            // });
        },
        // 添加最后的保存事件
        addEvent: function() {
            if(this.event.name) {
                this.events.push(this.event);
                this.event = { name: '', description: '', date: '' };
            }

            // 如果你想 Post 提交到后台，你可以这么写
            // this.$http.post('api/events', this.event).success(function(response) {
            //   this.events.push(this.event);
            //   console.log("Event added!");
            // }).error(function(error) {
            //   console.log(error);
            // });
        },
        // 删除已经添加的列表
        deleteEvent: function(index) {
            if(confirm("确定要删除嘛？")) {
                // model 里的 $remove 方法,为啥不用 $delete ? 思考
                this.events.$remove(index);
            }

            // 如果你想从数据库里删除，可以这么写
            // this.$http.delete('api/events', index).success(function(response) {
            //   this.events.$remove(index);
            // }).error(function(error) {
            //   console.log(error);
            // });
        }
    }
});

```
上面我们接触了这几个 api: 属于数据类型的data,methods，属于 DOM 类型的 el,属于生命周期的 ready,events(注册事件回调),以及实列方法里的 $set，$remove。这几个只是众多 api 里的一点点，还有很多 api 需要花时间去了解下。

这个代码在 Github 上：https://github.com/dyygtfx/Learn-Vue，直接 Down 下拉在浏览器打开就可以看见效果了。

最后，感觉 Vue 的 api 好简洁，概念也不像 React.js 那样难理解，如果你讨厌 React.js 可以试试 Vue，如果你初学 react，可以看看我之前学习时的一些小小 [DEMO](https://github.com/dyygtfx/Learn-React)



