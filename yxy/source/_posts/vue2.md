---
title: 'typescript总结笔记（8）常见面试题'
date: 2018-03-01 19:20:13
tags: 
  - vue
categories:
  - 程序员的自我修养
---

  <meta name="referrer" content="no-referrer">
## v-html

## computed 和 watch 

### computed 和 watch 的区别

1. computed有缓存，若data不改变是不会重新计算。watch
2. watch的深度监听
```js
computed: {
    fullname () {
      return this.firstname + ' ' +this.secondname
    },
    computedObj1 () {
      console.log('computedObj1 is change!')
      return this.obj
    },
    compuptedObj2:{
      get() {
        return this.num * 2
      },
      set(value) {
        this.num = value / 2
      }
    },
  },
```

```js
watch: {
	watchObj : {
    immediate: true,
    deep: true,
    handler (new) {
      
    }
  },
  watchNum (oldVal,newVal) {
    
  }
}
```



## style 和 class

## v-if 和 v-show 的区别

## v-for 

可以遍历数组和对象

### **为什么要使用key？**

`v-for`和`v-if`为什么不能一起使用？
`v-for`的优先级比`v-if`高，所以列表`item`很多的时候，会先将所有列表`item`渲染出来，然后再一个个的`v-if`判断。对性能影响会较差。

### event的使用，$event

在`vue`中，的`$event`是原生的，事件被挂载到当前元素上面了。
事件修饰符：
在`vue`中的事件是有修饰符的，我最常用的是@click.stop，可以阻止事件冒泡。
事件修饰符也可以串联写。
![](https://upload-images.jianshu.io/upload_images/20892169-5f2b77e00642d85b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
按键修饰符：
![](https://upload-images.jianshu.io/upload_images/20892169-0bf663105b13f7cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## v-model

包括 表单的`v-model` 和 自定义`v-model`

### 1.  表单的`v-model`

```html
<input type = "text" v-model="name"/>
<div>{{name}}</div>
```

是下面写法的语法糖，相当于：

```html
<input type = "text" @input="name = $event.target.value" :value="name">
<div>{{name}}</div>
```

### 2. 自定义组件`v-model`

`vue2`和`vue3`写法略微不同



## 父子组件通信 props $emit

### 父传子

父组件

```vue
<TableChoise @deleteItem="deleteItemEvent" :list="Lists"/>

```

js部分

```javascript
deleteItemEvent(item){
	console.log(item)
}
```

子组件

```html
<div @click="deleteItemSon($event,list)"></div>
```

js部分

```js
props: {
	Lists: {
		type: Array,
		default() {
			return []
		}
	}
}
// props: [Lists]
```

### 子传父

```js
methods: {
	deleteItemSon ($event,list) {
    this.$emit('deleteItem',list)
  }
}
```



## 组件间的通信 自定义事件

### 1. eventBus 

### 2. 自定义事件

`event.\$on('自定义事件名',触发的事件) ` 定义自定义事件
`event.\$off('自定义事件名')`  卸载自定义事件
`event.\$emit('自定义事件名',参数) `触发自定义事件

### 3. 依赖注入 provider/inject

**（1）写法**
根组件中：

```js
  // 依赖注入
  data () {
        return {
            message: 'aaaaaaaaa'
        }
  }
  provide () {
    return {
      message: this.message, // 不会使依赖有响应
      // message: computed(() => this.message) 
     // vue3可以通过导入computed去实现响应式
    }
  },
```
任意子孙组件中：
```js
inject: ['message'],
```
依赖注入默认不是相应式的。

### **vue2.0如何实现响应式的依赖注入？**

两种方法，一种可以将根组件的this当做依赖传入，然后用this调用响应式变量。
另一种可以将依赖当做函数传入
父组件

```js
provide () {
    return {
      parents: this  // 第一种方法
      // vue的provide虽然不是响应式的，但可以将响应式的this传入后，使用this去调用。
      parentName: () => this.name // 第二种方法
    }
  },
```
任一子孙组件
```js
  inject: ['parents','parentName'],
```
子孙组件vue模板
```vue
  <input type = "text" value = "" v-model = "parents.message"/>
{{message}}
<!--  用函数依赖注入时，this不能省略  -->
{{ this.parentName() }}  
```
也可以看
官网提供的实例：[点击查看](https://sfc.vuejs.org/#eNqNUs1OwzAMfhUrl4LEmntVkBAHTjwB4VBab83U/ChJC9LUd8dp2m5jE0yqlNr+/Nn+7AN7tjYfemQFK33tpA1PQktljQvw0squga0zCrKcT1aEZivgALVRtg/YwDjjUlxo/J4QDW6rviOk0DCBjUYdfEGpiX18iJGmCtXdfUIBOAy904sFoND7aocFZC12nSH+6B3jk9KtM4Ns8AaG0Eqfz9YpjdD0lXxVgIyAynZVQLIASqlpThg2yjTYPQq2cLAUTsNwMkq+JrIHloTaqMrme280qTz1JOaAF4ykSI0IRtpFW7A2BOsLzv22joLvfW7cjtNf7nodpMIcvdp8OvPl0RGxYJMMMwcn54Bu41A36ND9xfkLesG7iEOjrPu/diqvrtLN+b0cXfPR3HAUaYpj5m27OSl+uYHzNs56v9aO1HusQwHv2bzh7OPf+nZ6AN5SBgQDu1gV6liWxjosBwjjdG4ljymnfY4/qTg3rw==)
响应式依赖注入：[点击查看](https://juejin.cn/post/7051269498430554125)

### 组件生命周期

created mounted有什么区别？
created是创建了变量，js中存在了template，但还没有开始渲染
mounted是网页已经绘制完成了，已经渲染了
父子组件的生命周期钩子调用顺序？

### 自定义v-model

首先，要知道input 和 change两个事件的区别，input是在文本框的值改变时立即触发，而change是在文本框失去焦点时触发。

vue2.2+版本后，新增加了一个model选项，model选项允许自定义prop和event。官方原文是这样的：允许一个自定义组件在使用 v-model 时定制 prop 和 event。默认情况下，一个组件上的 v-model 会把 value 用作 prop 且把 input 用作 event，但是一些输入类型比如单选框和复选框按钮可能想使用 value prop 来达到不同的目的。使用 model 选项可以回避这些情况产生的冲突。

vue2和vue3写法不同。

### $nextTick
因为vue是异步框

架，在data改变后，dom不一定会立即渲染。
如果此时想去获取dom中的数据，可能会获取不到。
但如何知道开始渲染了？就在nextTick中获取

```
    this.$nextTick(function() {
      console.log('dom done')
    })
```
### 动态/异步组件

**动态组件：**
有时候不知道要加载什么组件，可以动态加载

```vue
<component :is="data中定义的变量，可以根据具体情况修改">
```
**异步组件：**
很常用。
内网开发的对内部人员用的网站，像echarts这种很大的组件，会严重影响性能。因为是vue是单文件实现多页切换，首页可能用不到图表也去加载的话，就会影响性能，此时就可以去异步加载。

```js
import { defineAsyncComponent } from 'vue'
xxx: defineAsyncComponent(()=>{
  import('xxx')
})
```
```vue
<xxx>
```



### slot

作用域插槽
具名插槽
![](https://upload-images.jianshu.io/upload_images/20892169-8a817b18221ca77a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### keep-alive

```js
<!-- aaacomponent可以被缓存，bbbcomponent不能被缓存，exclude优先级高于include，exclude和include内部可以写正则去控制 -->
<keep-alive include="aaacomponent" exclude="bbbcomponent">
  <aaacomponent />
  <bbbcomponent />
</keep-alive>
```
### mixin

多个组件有相同的逻辑，可以放入mixin中。
组件中：

```js
import myMixin from './mixin'
export default {
  mixins: [myMixin], // 可以引入多个minxin,[myMixin1, myMixin2, myMixin3,...]
}
```
在mixin.js中
```js
export default {
    data () {
      return {
        mixinData: 'mixinData'
      }
    },
    methods: {
      
    },
  }
```
缺点：
1. 变量来源不明确，引的多了不知道从哪儿来的
2. 多个`mixin`引入，如果是不同人开发的话，很有可能里面有变量名冲突。
3. `mixin`有可能出现多对多的关系，就是多个组件引入了一个`mixin`，一个组件还可以引入多个mixin，代码一旦复杂度提升，就会开始头疼了。。。
  vue3和vue2的区别
  v-model



```html
<template>
  
</template>
```



```js

export default {
  computed: {
    // 写法1
		fullname () {
      return this.firstname + this.lastname
    }
    // 写法2
    computedObj: {
    	get () {
  			return this.num * 2
			}
			set () {
        
      }
  	}
  }
}
```

