---
title: '【闭包的典型运用】防抖与节流'
date: 2019-03-17 12:13:35
tags:
  - js
  - lv2
categories:
  - 程序员的自我修养
---
<div id="cnblogs_post_body" class="blogpost-body cnblogs-markdown">
<p>闭包的典型应用就是函数防抖和节流，本文详细介绍函数防抖和节流的应用场景和实现。</p>
<h3 id="函数防抖debounce">函数防抖（debounce）</h3>
<p>函数防抖，就是指触发事件后，在 n 秒后只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数的执行时间。</p>
<blockquote>
<p>简单的说，当一个动作连续触发，只执行最后一次。</p>
</blockquote>
<p>打个比方，坐公交，司机需要等最后一个人进入才能关门。每次进入一个人，司机就会多等待几秒再关门。</p>
<h3 id="函数节流throttle">函数节流（throttle）</h3>
<blockquote>
<p>限制一个函数在一定时间内只能执行一次</p>
</blockquote>
<p>举个例子，乘坐地铁，过闸机时，每个人进入后3秒后门关闭，等待下一个人进入。</p>
<h3 id="常见的应用场景">常见的应用场景</h3>
<h4 id="函数防抖debounce的应用场景">函数防抖（debounce）的应用场景</h4>
<p>连续的事件，只需触发一次的回调场景有：</p>
<ul>
<li>搜索框搜索输入。只需要用户最后一次输入完再发送请求</li>
<li>手机号、邮箱格式的输入验证检测</li>
<li>窗口大小的 resize 。只需窗口调整完成后，计算窗口的大小，防止重复渲染。</li>
</ul>
<h4 id="函数节流throttle的应用场景">函数节流（throttle）的应用场景</h4>
<p>间隔一段时间执行一次回调的场景有：</p>
<ul>
<li>滚动加载，加载更多或滚动到底部监听</li>
<li>谷歌搜索框，搜索联想功能</li>
<li>高频点击提交，表单重复提交</li>
<li>省市信息对应字母快速选择</li>
</ul>
<h3 id="实现原理">实现原理</h3>
<h4 id="函数防抖debounce-1">函数防抖（debounce）</h4>
<p><strong>简单实现：</strong></p>
```js
const debounce = (func, wait) => {
    let timer
    return () => {
		clearTimeout(timer)
        timer = setTimeout(func, wait);
    }
}
```
<p>函数防抖在执行目标方法时，会等待一段时间。当又执行相同方法时，若前一个定时任务未执行完，则 清除掉定时任务，重新定时。</p>
<p><strong>封装：</strong></p>
```js
function debounce(fn, delay = 500) {
    // timer 是在闭包中的
    let timer = null;
    
    return function() {
        if (timer) {
            clearTimeout(timer)
        }
        timer = setTimeout(() => {
            fn.apply(this, arguments)
            timer = null
        }, delay)
    }
}

// test debounce 返回一个函数
input1 = document.getElementById('input1')
input1.addEventListener('keyup', debounce(function () {
    console.log(input1.value)
}, 600))
```
<p><strong>绑定事件解释：</strong>addEventListener 第一个参数是监听的事件，第二个参数是对应事件的回调函数。将 debounce 函数作为回调函数，这个 debounce 回调函数返回一个防抖之后的函数，因此实现了防抖的功能。</p>
<p><strong>防抖解释：</strong>当 按下某个键的时候触发 <code>keydown</code> 事件，并执行回调。timer 默认为 null，在 return 的函数中定时器 timer 被赋值，如果在 delay 延迟之内再次触发了 keydown 事件，那么 timer 就会被重置为null...，当用户输入完成之后（delay 时间已过），那么就会触发 debounce 中的回调函数，也就是 keydown 最终要执行的事件。</p>
<h4 id="函数节流throttle-1">函数节流（throttle）</h4>
<p><strong>简单实现</strong></p>

```js
const throttle = (func, wait) => {
    let timer;
    
    return () => {
        if (timer) {
            return
        }
        timer = setTimeout(() => {
            func();
            timer = null
        }, wait)
    }
}
```

<p>函数节流的目的，是为了限制函数一段时间内只能执行一次。因此，通过使用定时任务，延时方法执行。<strong>在延时的时间内，方法若被触发，则直接退出方法。</strong>从而实现一段时间内只执行一次。</p>
<p><strong>封装：</strong></p>
```js
function throttled(fn, delay = 500) {
    let timer = null
    return function (...args) {
        if (!timer) {
            timer = setTimeout(() => {
                fn.apply(this, args)
                timer = null
            }, delay);
        }
    }
}
// test
let div1 = document.getElementById('div1')
div1.addEventListener('drag', throttle(function(e) {
    console.log(e.offsetX, e.offsetY)
}, 100))
```
<p><strong>解释：</strong>如果 timer 存在，那就直接返回，不再往下执行了。这样就实现了一段时间内执行一次的目的。</p>
<h4 id="异同比较">异同比较</h4>
<p><strong>相同点：</strong></p>
<ul>
<li>都可以通过使用 <code>setTimeout</code> 实现</li>
<li>目的都是，降低回调函数的执行频率，节省计算资源</li>
</ul>
<p><strong>不同点：</strong></p>
<ul>
<li>函数防抖，是在一段连续操作结束之后，处理回调，利用 clearTimout 和 setTimeout 实现。函数节流，是在一段连续操作中，每一段时间只执行一次，在频率较高的事件中使用来提高性能。</li>
<li>函数防抖关注一段时间内连续触发，只在最后一次执行；而函数节流侧重于在一段时间内只执行一次。</li>
</ul>

节流其实还有另一种不用定时器的方法。
新的时间戳 - 旧的时间戳 > delay时间的话，执行方法。否则将此时的时间戳赋值为旧的时间戳
```js
function throttled1(fn, delay = 500) {
    let oldtime = Date.now()
    return function (...args) {
        let newtime = Date.now()
        if (newtime - oldtime >= delay) {
            fn.apply(null, args)
            oldtime = Date.now()
        }
    }
}

```
</div>