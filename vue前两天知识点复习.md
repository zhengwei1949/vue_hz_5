# 知识点复习
## vue解决了以前的什么问题
- 操作dom太麻烦了，各种选择器筛选器
- 只要页面的结构变了，之前写代码的模式下所有的js全部失效必须重写，极其难以维护
## vue写代码的思想
- mvvm
    + mvvm = m(数据) + vm(vue对象实例) + v(视图)
- 操作数据而不是操作视图
- 因为数据是响应式的
    + 当数据变了，视图自然会跟着进行改变
## 内置指令
- v-cloak 解决插值表达式闪烁的问题
- v-text 作用和插值表达式一样，不推荐使用，太繁琐
- v-html 当你的数据字符串是一个html片段的时候用这个
- v-once 默认数据是响应式的，所以每次数据变了，都要遍历当前这块需不需要去更新，如果你确定只需要渲染一次，可以用v-once，提升性能
- v-model 用于表单控件值，可以实现双向数据绑定
- v-on 用来绑定事件
    + 语法：v-on:事件类型="回调函数名字"
    + 简写方式 @事件类型="回调函数名字"
    + 事件对象$event
- v-bind
    + 动态绑定属性
    + 语法：v-bind:属性名="data中的变量"
    + 简写：:属性名="data中的变量"
    + 样式相关
        1. v-bind:class
        2. v-bind:style
- v-if
    + 注册登录切换用这个
- v-show
    + 频繁切换用这个
- v-for
    + 遍历用的
    + 一定要加上:key属性
## 修饰符
    + 事件修饰符
        1. @keyup.prevent 阻止默认行为
        2. @keyup.stop 阻止冒泡
    + 按键修饰符
        1. @keyup.enter
        2. @keyup.13
        3. Vue.config.keyCodes.f2 = 113 
    + 表单控件修饰符
        + v-model.number
        + v-model.trim
        + v-model.lazy
## 自定义指令
    + vue内置的指令可能在开发的时候不够用，这时候，我们可以考虑自己自定义指令

```html
<div v-指令的名字="值"></div>
```

```js
Vue.directive('指令的名字',{
    //绑定 指令绑定到当前这个元素的时候就会触发bind钩子函数执行
    //el代表的是当前指令所在的元素
    //binding可以拿到调用指令的时候的值
    bind:function(el,binding){

    },
    //插入 指令所在的元素被添加到DOM树上的时候 
    inserted:function(el,binding){

    }
})
```

## 计算属性
- 如果我们在模板当中需要一个值，它是通过data中其他数据计算出来的，我们就可以用计算属性
- 计算属性是响应式的，当依赖的数据变了的时候，计算属性的值也会跟着变
- 语法

```html
<div id="app">
    <div>{{msg}}</div>
    <div>{{reverseMsg}}</div>
</div>
```

```js
new Vue({
    el:'',
    data:{
        msg:'hello'
    },
    methods:{
        
    },
    computed:{
        reverseMsg:function(){
            return this.msg.split('').reverse().join('');
        }
    }
})
```

## 计算属性和方法的区别
- 当我们需要给模板中的按钮添加点击事件的时候，我们需要在methods中添加方法
- 当我们在页面中有一个数据，它是依赖于另外的数据得到的时候 ，可以用计算属性
- 计算属性不能加括号，否则会报错
- 计算属性的数据有缓存

## 侦听器
- 我们可以通过侦听器来监视data中的数据的改变
- 语法

```html
<div id="app">
    <input v-model="msg">
</div>
```

```js
new Vue({
    el:'',
    data:{
        msg:'hello'
    },
    methods:{
        
    },
    watch:{
      msg:function(now){
        console.log('msg值变了，现在的值是'+now);
      }
    }
})
```

## 过滤器
- 当我们需要对数据的格式进行转换的时候，我们可以考虑用过滤器
- 语法

```html
<div id="app">
    {{data | dateFormat}}
</div>
```

```js
Vue.filter('dateFormat',function(str){
  var date = new Date(str);
  var y = date.getFullYear();
  var m = date.getMonth()+1;
  var d = date.getDate();
  return y + '-' + m + '-' + d;
})
new Vue({
    el:'',
    data:{
        str:'2525609975000'
    },
    methods:{
        
    }
})
```

## 方法、计算属性、侦听器的使用场景区分
- 事件用方法
- 繁琐的插值表达式用计算属性
- 异步更新数据用侦听器

## 生命周期钩子函数
- beforeCreate   el,data,methods都没准备好，这个阶段初始化生命周期钩子函数
- created       data和methods准备好了（一般会在这块去请求ajax数据或者本地存储数据）
- beforeMount  找到了模板
- mounted     模板和数据同步了，可以去操作DOM了（一般会在这一块去使用第三方插件的时候去初始化dom）
    + https://echarts.baidu.com/tutorial.html#5%20%E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B%20ECharts
- beforeUpdate 虚拟dom更新完毕
- updated      真实dom更新完毕
- beforeDestroy 我们可以在离开之前做一些清理性的工作，比如对定时器清除
- destroyed vue实例销毁，什么也做不了


```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>

<body>
  <div id="app">
    <h1>{{ message }}</h1>
    <input type="text" v-model="message">
    <button @click="fn">按钮点击</button>
  </div>
  <script src="js/vue.js"></script>
  <script type="text/javascript">
    //如何触发beforeUpdate,updated --> 在控制台执行app.message = '其他的值'
    //如何触发beforeDestroy destroyed --> 在控制台执行app.$destory()
    var vm = new Vue({
      el: '#app',
      data: {
        message: "hello vue"
      },
      methods: {
        fn() {
          console.dir('methods方法执行了');
        }
      },
      //组件创建之前
      beforeCreate() {
        console.log('beforeCreate el,data,methods都没准备好');
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        this.fn();
        console.dir('>==========================================<');
      },
      //组件创建完毕
      created() {
        console.log('created data和methods准备好了');
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        console.log('页面中h1里面的值是：'+document.querySelector('#app h1').innerHTML);
        this.fn();
        console.dir('>==========================================<');
      },
      // 组件挂载之前
      beforeMount() {
        console.dir('beforeMount 找到了模板');
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        console.log('页面中h1里面的值是：'+document.querySelector('#app h1').innerHTML);
        this.fn();
        console.dir('>==========================================<');
      },
      // 组件挂载完毕
      mounted() {
        console.dir('mounted 模板和数据同步了，可以去操作DOM了');
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        console.log('页面中h1里面的值是：'+document.querySelector('#app h1').innerHTML);
        this.fn();
        console.dir('>==========================================<');
      },
      // 组件更新之前
      beforeUpdate() {
        console.dir('beforeUpdate 虚拟dom更新完毕');
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        console.log('页面中h1里面的值是：'+document.querySelector('#app h1').innerHTML);
        this.fn();
        console.dir('>==========================================<');
      },
      // 组件更新完毕
      updated() {
        console.dir('updated 真实dom更新完毕');
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        console.log('页面中h1里面的值是：'+document.querySelector('#app h1').innerHTML);
        this.fn();
        console.dir('>==========================================<');
      },
      // 组件销毁之前
      beforeDestroy() {
        console.dir('beforeDestroy 我们可以在离开之前做一些清理性的工作，比如对定时器清除');
        console.dir("%c%s", "color:red", "el : " + this.$el);
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        this.fn();
        console.dir('>==========================================<');
      },
      // 组件销毁完毕
      destroyed() {
        console.dir('destroyed 实例销毁，什么也做不了');
        console.log("el : ");
        console.dir(this.$el);
        console.log("data : ");
        console.dir(this.$data);
        console.log("message: " + this.message);
        this.fn();
        console.dir('>==========================================<');
      }
    })
  </script>
</body>

</html>
```




