---
title: '【vue3】总结'
date: 2023-02-27 18:16:25
tags: vue
categories: 
  - 程序员的自我修养
---

<meta name="referrer" content="no-referrer">

## Vue3和Vue2的区别
+ Vue3支持2的大多数写法
+ 更好的支持Typescript
+ 使用Proxy代替了defineProperty
+ 重写了虚拟dom的实现，及Tree Shaking的实现。
+ 从Option api变成Composition api
  + setup
     + ref和reactive
     + computed和watch
     + provide和inject
     + 生命周期的更改
  + 新组件
     + Fragment
     + Teleport
     + Suspense
  + 其他
     + 全局api的修改
     + 原来的全局api转移到应用对象
     + 模板语法修改

## 一、新建
```
npm init vue
```
或
```
npm create vite
```

## 二、Composition API（组合式API）和Options API
#### 1. Composition API带来了什么？
+ 代码组织的更加整齐
+ 逻辑复用做的更加清晰
+ 更好的类型推导
![option api和composition api的对比](https://upload-images.jianshu.io/upload_images/20892169-a033ffcf9ab7f6a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

composition api可以将声明的变量和方法等放在一起，这样会使代码更整齐，也方便逻辑复用。

 #### 2. 如何选择？
+ 最好不要共用Options API和Compositions API
+ 小型项目就用Options API，逻辑复杂用Composition API
+ Compositon API 就像 Hooks在React

## 三、 组合式api
### 1. ref
ref用来定义**基本数据类型**的响应式。

*备注：ref也可以用来定义对象或数组类型数据，它内部会自动通过 reactive 转为代理对象。*
```js
const aaa = ref({
  name: {
    age: 12
  },
  age: 13333,
  items: [{
    id:2,
    sex:'f'
  }]
})
function changeName(){
  aaa.value.items[0].id++
}
```
```js
<template>
  <div class="greetings">
   <div>{{ aaa.items[0].id }}</div>
   <div class="btn" @click="changeName()">点击</div>
  </div>
</template>
```
在setup中使用value去获取值，在vue模板中直接使用变量名去获取。
```js
<script setup lang = "ts">
  let count = ref(0)
  console.log(count.value)
</script>
```
```js
    <div>{{ count }}</div>
```

#### ref 可以用来获取dom元素
在dom上绑定ref=xxx，在 setup 中定义

```js
let xxx = ref(null)
```
xxx就是dom元素。
当然，绑定在组件上就可以获取组件的元素和方法。
### 2. reactive
reactive用来定义多个对象的响应式。
```js
const user = reactive({
  name: "小明",
  age: 20,
  wife: {
    name: "小甜甜",
    age: 21,
    cars: ["奔驰", "宝马", "奥迪"],
  },
});
```
### 3、reactive和ref的区别
#### 3.1\. 定义数据角度对比：
`ref` 用来定义：基本类型数据
`reactive` 用来定义：引用类型，例如对象、或[数组](https://so.csdn.net/so/search?q=%E6%95%B0%E7%BB%84&spm=1001.2101.3001.7020)类型的数据
#### 3.2. 原理角度对比：
`ref`通过 Class 的 get 与 set 来实现响应式的（数据劫持）
`reactive` 通过使用 Proxy 来实现响应式（数据劫持），并通过Reflect 操作源对象内部的数据。
#### 3.3. 使用角度对比：
`ref` 定义的数据：操作数据需要 .value,读取数据时模版中直接读取不需要 .value
`reactive` 定义的数据：操作数据与读取数据，均不需要 .value
#### 3.4. reactive和ref的关系
如果用ref对象/数组，内部会自动将对象/数组转为reactive的代理对象。
### 5. setup
#### 5.1. setup的执行时机
在vue2的beforeCreate的生命周期之前执行，且只执行一次。
setup执行时，当前组件未创建，所以没有this
#### 5.2. setup(props,context)
`setup`函数的第一个参数为`props`，`props`不可以解构，如果解构会导致`props`失去响应式。
`setup`函数的第二个参数为context，context可以解构，解构后为
```js
{ attrs, slots, emit, expose }
```
即：
```js
setup(props, context) {
    // 透传 Attributes（非响应式的对象，等价于 $attrs）
    console.log(context.attrs)

    // 插槽（非响应式的对象，等价于 $slots）
    console.log(context.slots)

    // 触发事件（函数，等价于 $emit）
    console.log(context.emit)

    // 暴露公共属性（函数）
    console.log(context.expose)
  }
```
或者：
```js
  setup(props,context) {
    ...
  }
```
都是ok的。
在<script setup></script>中，引入defineProp使用props
```js
import { defineEmits, defineProps } from "vue";
let props = defineProps<{
  modelValue: string;
}>();
let emit = defineEmits();
```
### 6. computed
```js
<script setup lang="ts">
import { ref, computed } from "vue";
let firstname = ref("john");
let lastname = ref("watson");
let fullname = computed(() => {
  return `${firstname.value} ${lastname.value}`;
});
</script>
```
### 7. watch
watch有三个参数，第一个是监听的变量。（如果有多个变量想同时监听，可以使用数组）
第二个是函数，可以监听到旧值与新值。
第三个参数可以配置deep、immediate等

```js
<script setup lang="ts">
import { ref, computed, watch } from "vue";
let firstname = ref("john");
let lastname = ref("watson");
let fullname = computed(() => {
  return `${firstname.value} ${lastname.value}`;
});
// watch三个参数，第一个是监听的变量。（如果有多个变量想同时监听，可以使用数组）第二个是函数，可以监听到旧值与新值，第三个参数可以配置deep、immediate等
watch(
  fullname,
  (newVal, oldVal) => {
    console.log(`newFullName=${newVal}`);
    console.log(`oldFullName=${oldVal}`);
  },
  { deep: true }
);
</script>
```
### 8. watchEffect
立即运行一个函数，同时响应式地追踪其依赖，并在依赖更改时重新执行。
第一个参数是一个方法，方法的参数可以用来清理无效的副作用，清理回调会在该副作用下一次执行前被调用。
第二个参数是一个可选的选项，可以用来调整副作用的刷新时机或调试副作用的依赖。
#### flush的值
默认情况下，侦听器将在组件渲染之前执行。

设置 `flush: 'post'` 将会使侦听器延迟到组件渲染之后再执行。
在某些特殊情况下 (例如要使缓存失效)，可能有必要在响应式依赖发生改变时立即触发侦听器。这可以通过设置 `flush: 'sync'` 来实现。（**该设置应谨慎使用，因为如果有多个属性同时更新，这将导致一些性能和数据一致性的问题。**）
```js
<script setup lang="ts">
import { ref, watchEffect } from "vue";
let firstname = "john";
let lastname = ref("watson");
let fullname = ref("");
watchEffect(() => {
  fullname.value = firstname + lastname.value;
});

</script>
```
#### 所以它经常被用来：
+ 观察反应变量的变化
+ 允许开发者执行副作用
+ 提供一种取消副作用的方法（以防状态无效）
#### watch和watchEffect之间的区别：
+ watch可用于延迟触发副作用（watchEffect总是立即的）。
+ watchEffect自动监视任何状态更改的更改（watch必须提供一个或多个要监视的变量）。
+ watch提供对当前值和先前值的访问。
+ watch也可以用来监听非响应式的数据，但写法比较麻烦。

#### vue3 的 hooks（vue2的mixin）
vue3在共享数据的时候，用的是`hooks`的方法。
比如我想实现一个方法，在鼠标移动时去获取鼠标的坐标
定义一个js/ts模块，封装useMousePosition
```js
import { onMounted, ref } from "vue";
export default function () {
  const x = ref(-1);
  const y = ref(-1);
  const clickHandler = (event: MouseEvent) => {
    x.value = event.pageX;
    y.value = event.pageY;
  };
  onMounted(() => {
    window.addEventListener("mousemove", clickHandler);
  });
  return { x, y };
}

```
在某个vue组件中，直接用引入的方式调用
```js
<script setup lang="ts">
import useMousePosition from "../hooks/useMousePosition";
let x = useMousePosition().x;
let y = useMousePosition().y;
</script>
<template>
  <div>{{ x }},{{ y }}</div>
</template>
```
### 9. toRefs 
toRefs 可以将每一个响应式对象变为一个普通对象，该普通对象的每一个property都是一个ref。
#### toRefs解决问题的场景：
使用`reactive`去定义响应式对象的时候，如果对象层级较多，写起来会显得不简洁，而如果使用`...`解构赋值的话，对象的属性又会失去响应式。

为了解构赋值的时候不失去响应式，可以使用`toRefs`将响应式对象内部的所有变量变成响应式的。

不过这种方式现在在`script setup`中基本没有好的实现方法，只能在setup()里面用用。。
```js
<script lang="ts">
// 现在暂时没什么好方法，去在script setup里使用toRefs，所以用这种方式来写
import { toRefs, reactive } from "vue";

export default {
  setup() {
    const personReactive: Object = reactive({
      name: "john",
      age: 133,
    });
    const personObj = toRefs(personReactive);
    return {
      ...personObj,
    };
  },
  data() {},
};
</script>
```
### 10. toRef()
`toRef(obj,'propname')`可以将reactive的某一个属性转换为Ref
```js
<script setup lang="ts">
import { reactive, isRef, toRef } from "vue";
const state = reactive({
  name: "john",
  age: 20,
  other: {
    employee: true,
  },
});
let name = toRef(state, "name");
function clickHandler() {
  name.value += name.value;
}
</script>
```
```js
<template>
  <div>name:{{ name }}</div>
  <div class="btn btn-confirm" @click="clickHandler">确认</div>
</template>
```
### 11. isRef()
检查某个值是否为ref
```js
let foo: unknown;
if (isRef(foo)) {
  // foo 的类型被收窄为了 Ref<unknown>
  foo.value;
}
```
## 四、组合式api 进阶
### 1. shallowReactive和shallowRef

`shallowReactive`包装对象，只让对象的第一层有响应。
`shallowRef`包装对象，是生成一个非响应式的对象，就是说将对象重新赋值是可以有响应式的，但为对象的任意属性赋值都是没有响应式的。
（可是就我自己实验观察。。好像ShallowRef也可以触发第一层的响应）
`shallowRef`包装基本类型，和Ref用法一样。
shallowRef官网的例子
```js
const state = shallowRef({ count: 1 })

// 不会触发更改
state.value.count = 2

// 会触发更改
state.value = { count: 2 }
```
shallowReactive 官网的例子
```js
const state = shallowReactive({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 更改状态自身的属性是响应式的
state.foo++

// ...但下层嵌套对象不会被转为响应式
isReactive(state.nested) // false

// 不是响应式的
state.nested.bar++

```
因为有疑惑，所以自己的代码先不贴了。
#### 2. triggerRef()
强制触发依赖于一个[浅层 ref](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref) 的副作用，这通常在对浅引用的内部值进行深度变更后使用。
```js
function triggerRef(ref: ShallowRef): void
const shallow = shallowRef({
  greet: 'Hello, world'
})
// 触发该副作用第一次应该会打印 "Hello, world"
watchEffect(() => {
  console.log(shallow.value.greet)
})

// 这次变更不应触发副作用，因为这个 ref 是浅层的
shallow.value.greet = 'Hello, universe'

// 打印 "Hello, universe"
triggerRef(shallow)
```
#### 3. readonly、shallowReadonly、isReadonly
`readonly()`接受一个对象 (不论是响应式还是普通的) 或是一个`ref`，返回一个原值的只读代理。只读代理是深层的。
##### readonly()
```js
const original = reactive({ count: 0 })

const copy 
= readonly(original)

watchEffect(() => {
  // 用来做响应性追踪
  console.log(copy.count)
})

// 更改源属性会触发其依赖的侦听器
original.count++

// 更改该只读副本将会失败，并会得到一个警告
copy.count++ // warning!
```
`shallowReadonly`只有表层是只读的
`isReadonly`判断某个变量是不是只读的

##### shallowReadonly
```js
const state = shallowReadonly({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 更改状态自身的属性会失败
state.foo++

// ...但可以更改下层嵌套对象
isReadonly(state.nested) // false

// 这是可以通过的
state.nested.bar++
```
#### 4. toRaw()  markRaw()
toRaw 把代理的响应式对象变为了普通对象
```js
<script setup lang="ts">
import { reactive, toRaw, markRaw } from "vue";
// toRaw 把代理的响应式对象变为了普通对象
// markRaw 将一个对象标记为不可被转为代理。返回该对象本身。
interface PersonInfo {
  [props: string]: any; // 我为了省事儿这么写的，一般还是严谨些写阿。。
}
const originObj: PersonInfo = reactive<PersonInfo>({
  name: "john",
  age: 20,
});
let toRawObj = toRaw(originObj);
function clickHandler() {
  toRawObj.age--;
  console.log(toRawObj);
}
</script>
<template>
  <div>{{ toRawObj }}</div>
  <!-- <div>{{ markRawObj }}</div> -->
  <div class="btn btn-confirm" @click="clickHandler">确定toRaw</div>
  <div class="btn btn-confirm" @click="clickHandler">确定markRow</div>
</template>
```
markRaw 将一个对象标记为不可被转为代理。返回该对象本身。
```js
const foo = markRaw({})
console.log(isReactive(reactive(foo))) // false

// 也适用于嵌套在其他响应性对象
const bar = reactive({ foo })
console.log(isReactive(bar.foo)) // false
```
#### 5. customRef()
创建一个自定义的 ref，显式声明对其依赖追踪和更新触发的控制方式。
myRef接收一个值，返回customRef函数的执行 结果，这个函数接收2个参数，一个track（追踪）,一个trgger(触发)。
返回一个存储器对象，有个get和set方法，取值时执行get,赋值时执行set。
```js
function myRef(value){
  return customRef((track,trigger)=>{
    return {
      get(){
        //追踪
        track()
        return value;
      },
      set(newValue){
        value=newValue
      //触发更新
        trigger()
      }
    }
  })
}
```
官网写了一个防抖的例子。
```js
<script setup lang="ts">
import { reactive, customRef, ref } from "vue";
interface PersonInfo {
  [props: string]: any; // 我为了省事儿这么写的，一般还是严谨些写阿。。
}
const originObj: PersonInfo = reactive<PersonInfo>({
  name: "john",
  age: 20,
});
const keyword = useDebounceRef("abc", 500);
</script>
```
### 6. Fragment（碎片）和Teleport（瞬移）
`Fragment`：vue3不需要根标签了。
`Teleport`：让组件的html可以在父组件外的特定标签下面插入展示。（比如body）
```js
<template>
  <div class="btn btn-confirm" @click="showModal(true)">打开对话框</div>
  <Teleport to="body">
    <div class="modal-wrapper" v-if="isShowModal">对话框</div>
    <div class="btn btn-confirm" @click="showModal(false)">关闭对话框</div>
  </Teleport>
</template>
```
用 <Teleport to="body"></Teleport>标签将子组件内的一部分内容包裹，那么这部分内容就会在body中展示。
### 7. Suspense
比如页面里面要做一些异步操作，或者说调一些异步组件，这时候可能会出现等待的情况，那么在等待过程中，可以使用suspense渲染一些后备内容。
