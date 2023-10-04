---
title: react-hooks
date: 2023-03-14 17:11:14
tags:
---
## 一、基本介绍
react是基于数据是不可变的（每次setState都会返回一个新数据），这也是为什么需要setState()来更新数据而不能使用像vue的this.state = newState的形式更新数据的原因，其实你用this.state=newState确实可以改数据，但是react不知道数据变了。

## 二、useMemo、useCallback的执行时机对比

useMemo和useCallback都会在**组件第一次渲染的时候执行**，之后会在其**依赖的变量发生改变时再次执行**；并且这两个`hooks`都返回缓存的值，`useMemo`返回**缓存的变量**，`useCallback`返回**缓存的函数**。

## 三、useEffect 和 useMemo 区别
useEffect是在DOM改变之后触发，useMemo在DOM渲染之前就触发
useMemo是在DOM更新前触发的，就像官方所说的，类比生命周期就是[shouldComponentUpdate]
useEffect可以帮助我们在DOM更新完成后执行某些副作用操作，如数据获取，设置订阅以及手动更改 React 组件中的 DOM 等
不要在这个useMemo函数内部执行与渲染无关的操作，诸如副作用这类的操作属于 useEffect 的适用范畴，而不是 useMemo
在useMemo中使用setState你会发现会产生死循环，并且会有警告，因为useMemo是在渲染中进行的，你在其中操作DOM后，又会导致触发memo

## 三、useMemo
可以把它理解成vue里面的computed，是一种数据的缓存，而这个缓存依赖后面的第二个参数数组。

如果页面上展示的数据是通过某些state计算得来的一个数据，那么你每次这个组件里面无关的state变化引起的重新渲染，都会导致这个数据重新计算。

这时候就需要用`useMemo(()=>{}, [])`去包裹你的计算的方法体，这样那些无关的`state`改变引起的渲染不会重新计算这个方法体，而是返回之前计算的结果，达到一种缓存的效果。
```js
import "./App.css";
import React, { useState, useMemo } from "react";
```
```js
function renderColor() {
  // 获得随机颜色
  const r = Math.floor(Math.random() * 255);
  const g = Math.floor(Math.random() * 255);
  const b = Math.floor(Math.random() * 255);
  const a = Math.random();
  const color = `rgba(${r},${g},${b},${a})`;
  return color;
}
```
```js
function App() {
  const [n, setN] = useState(0);
  const [color, setColor] = useState("");
  const styleSheet = {
    backgroundColor: renderColor(),
  };
  // 使用useMemo创建有缓存的组件
  // 1. useMemo相当于vue computed
  // 2. useMemo必须有return的内容，所以在模板中调用是直接写 { TestComponent }
  // 3. 只有当color变化时，才会触发 TestComponent 的更新
  const TestComponent = useMemo(() => {
    console.log("子组件也更新了");
    return (
      <div className="button" style={styleSheet}>
        {color}
      </div>
    );
  }, [color]);
  const inputHandlerColor = (event) => {
    setColor(event.target.value);
    console.log("color");
  };
  const inputHandler = (event) => {
    setN(event.target.value);
    console.log("n");
  };
  return (
    <div>
      <input
        type="text"
        className="input_border"
        onChange={(event) => {
          inputHandler(event);
        }}
      />
      <input
        type="text"
        className="input_border"
        onChange={(event) => {
          inputHandlerColor(event);
        }}
      />
      <div className="button">{n}</div>
      {TestComponent}
    </div>
  );
}
export default App;
```

## 四、useCallback
useCallback跟useMemo比较类似，但它返回的是缓存的函数。

使用场景：有一个父组件，其中包含子组件，子组件接收一个函数作为props；通常而言，如果父组件更新了，子组件也会执行更新；但是大多数场景下，更新是没有必要的，我们可以借助useCallback来返回函数，然后把这个函数作为props传递给子组件；由于useCallback返回的函数实例在第二个参数，也就是依赖项未发生变化时不会被重新创建，因此，每次父组件的更新不会导致子组件内部的函数实例发生变化。
这样，子组件就能避免不必要的更新。

**useCallback一般要配合React.memo来使用：**
react的Hooks组件对props的浅比较是在memo里面比较的（类组件是在shouldComponentUpdate里面），如果没有memo，那么你使用useCallback就没啥意义，反而浪费性能（因为useCallback来包裹函数也是需要开销的）。因为子组件还是会重新渲染。

```js
function APP() {

    const [value, setValue] = useState(123)
    const [otherValue, setOtherValue] = useState(999)

    const changeValue = useCallback(() => {
        setValue(value => value+1)
    }, [])
    
    console.log('APP');

    return (
        <div>
            <div>与Message渲染无关的数据==={otherValue}</div>
            <br />
            <button onClick={() => setOtherValue(value => value-=5)}>改变无关的数据</button>
            <br />
            <br />
            <Message value={value} changeValue={changeValue} />
        </div>
    )
}
```
```js
const Message = memo(
    function Message({value, changeValue}) {
    
        console.log('Message');
    
        return (
            <div>
                <button onClick={changeValue}>改变有关数据</button>
                <p>与Message渲染有关的数据{value}</p>
            </div>
        )
    }
)
```
### React.memo与useMemo的区别和联系
在React的组件中，如果子组件没有被React.memo包裹，或者没有使用useMemo来处理props传递参数，那么**当父组件的任何值更新时，整个组件都将会进行重新渲染**，包括父组件下面的所有子组件。多数情况下对于子组件来说没有必要。毕竟不是父组件的每一次更新都需要修改子组件的值，而频繁的更新却会导致不需要更新的子组件被迫更新，造成资源的浪费。
针对上述问题，React提供了React.memo和useMemo。

#### React.memo：
React.memo()本质是一个高阶组件（HOC），高阶组件和高阶函数类似，高阶函数是接收一个函数，然后经过一些判断和处理后再返回这个函数。
对应到高阶组件，就是接收一个组件，然后经过一些判断和处理后再返回这个组件。
再回归到React.memo(), 这个高阶组件接收一个组件A作为参数并返回一个组件B，如果组件B的props没有改变，则组件B会阻止组件A重新渲染。A和B本质上是同一个组件，但A是否进行重新渲染，需要由Props是否发生改变来决定。
```js
// 父组件
import {useMemo, useState} from "react";
import ReactMemoChild from "./ReactMemoChild";

export const ReactMemoFather = () => {
    const [count, setCount] = useState(0);
    return (
        <>
            <p>按钮点击次数：{count}</p>
            <ReactMemoChild/>
            <button onClick={() => setCount(n => n + 1)}>按钮</button>
        </>
    );
}
```
```js
import React, {useMemo, useRef} from "react";

function ReactMemoChild() {
    const ref = useRef(0);
    return (
        <>
            <p>页面渲染次数：{ref.current++}</p>
         </>
    );
}

export default React.memo(ReactMemoChild);
```
#### useMemo：
React提供的一个hook函数，他不是高阶组件。
使用useMemo定义的变量，只会在useMemo的第二个依赖参数发生修改时才会发生修改。
使用useMemo时，应保证第一个参数函数里所使用的变量都出现在第二个依赖参数数组中，这样可以避免一些额外的错误。
```js
// 父组件
import {useState, useRef, useMemo} from "react";
import UseMemoChild from "./UseMemoChild";

export default function UseMemoFather() {
  const [count, setCount] = useState(0);
    const [times, setTimes] = useState(0);
    const useMemoRef = useRef(0);

    const incrementUseMemoRef = () => useMemoRef.current++;

    const memoizedValue = useMemo(() => incrementUseMemoRef(), [times]);

    return (
        <div>
            <div>
                <p>按钮点击次数：{count}</p>
                <button onClick={() => setCount(count + 1)}>按钮</button>

                <button onClick={() => setTimes(times + 1)}>
                    Force render
                </button>

                <UseMemoChild memoizedValue={memoizedValue}/>
            </div>
        </div>
    );
}

```
```js
// 子组件
interface PropType{
    memoizedValue: number
}

function UseMemoChild({memoizedValue}: PropType) {
    return (
        <div className="mt-3">
            <p className="dark:text-white max-w-md">
                I'll only re-render when you click <span className="font-bold text-indigo-400">Force render.</span>
            </p>
            <p className="dark:text-white">I've now rendered: <span className="text-green-400">{memoizedValue} time(s)</span> </p>
        </div>
    );
}
export default UseMemoChild;

```

![image-20230330145149288](/images/image-20230330145149288.png)