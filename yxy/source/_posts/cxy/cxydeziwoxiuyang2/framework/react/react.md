---
title: 'react18基础知识梳理'
date: 2021-03-25 13:03:49
tags: react
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">


## 思维导图

![](https://upload-images.jianshu.io/upload_images/20892169-c7559513f639bb9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 一、jsx
### jsx中使用js表达式
在jsx中js表达式是用`{}`括起来的，和vue一样。可以调用的有
1.常规变量
2.原生js
3.三元运算符
```javascript
const name="myname"
const getAge = ()=>{
	return 18
}
const flag = true

function App() {
  return (
    <div className="wrapper-box">
        <div className="content-box">
          {name}
        </div>
        <div className="content-box">
          {getAge()}
        </div>
        <div className="content-box">
          {flag ? 1:0}
        </div>
    </div>
)
```

### 列表渲染：map
使用Object.keys去获取obj列表，再用map循环
```javascript
        <div className="wrapper-obj-list">
            {Object.keys(obj).map((key)=><div key={key}>
              {obj[key]}
            </div>)}
        </div>
```
### style: 使用`\{\{\}\}`
```javascript
<div className="wrapper-style" style={{
          color: 'red',
          backgroundColor:'blue'
        }}>sdfsdfsdfsdfsdfdsfsdfsdfsdfdsfdsfsdfdsfsdfsdfsdfdsfsdfsdfsdfdsfdsfsdfdsfsdfdsfsdfsdfsdfdsfdsfsdfdsf</div>
```
### 条件渲染：三元表达式
模版太长时，可以使用括号包裹，并可以换行。
```javascript
const spanflag = true
```
```javascript
<div>{spanflag?(
          <span>span内的内容</span>
          ):null}
        </div>
```
### 模板精简
将重合的部分提出放入函数中，在App()中可以调用
```javascript
const getHTag = (n)=>{
  if(n == 1) {
    return <h2>数字是{n}</h2>
  } else if (n == 2) {
    return <h2>数字是{n}</h2>
  } else if (n == 3) {
    return <h2>数字是{n}</h2>
  } else {
    return <h2>数字不是123</h2>
  }
}
function App() {
  return (
    <div>
      {getHTag(3)}
    </div>
  );
}
```
### 其他注意事项
1. jsx标签必须闭合，只有一个根节点
2. jsx标签换行，必须加小括号
3. jsx属性名采用驼峰命名法，如
`class->className`
`for->htmlfor`

## 二、组件
### 1. 函数组件
+ 组件名称必须首字母大写
+ 组件必须有返回值，且返回值为jsx模板，什么jsx都不返回则为null
+ 使用函数名作为组件标签名。
```javascript
import "./App.css";

function DivTemplate() {
  return (
    <div>
      <span>函数式组件</span>
    </div>
  );
}
function App() {
  return <DivTemplate />;
}

export default App;

```
### 2. 类组件
+ 类组件类组件中必须使用render()方法去返回一个jsx结构，render方法必须要有返回值
+ 类组件必须继承React.Component，并引入react
```javascript
import "./App.css";
import React from "react";

class DivTemplate extends React.Component {
  render() {
    return <div>这是一个类组件</div>;
  }
}
function App() {
  return <DivTemplate />;
}

export default App;

```
### 3. 事件绑定
+ 事件格式为on+事件，函数组件可以直接调用方法，类组件调用用this.xxx方法
+ 传递自定义参数的话，如果同时需要使用事件对象e和自定义参数的话，需要在外层套一层箭头函数传入
```
// 函数组件绑定时间
function DivTemplateF() {
  const clickHandler = (e, msg) => {
    console.log("函数组件中的点击事件被触发了");
    console.log(`函数组件中：${e.target},${msg}`);
  };
  return (
    <div
      style={{
        width: "100%",
        height: "60px",
        lineHeight: "60px",
        backgroundColor: "#00ffff",
        textAlign: "center",
      }}
      onClick={(e) => {
        clickHandler(e, "this is msg");
      }}
    >
      点击触发函数组件事件
    </div>
  );
}
```

```javascript
// 类组件绑定事件
class DivTemplateC extends React.Component {
  clickHandler(e, str) {
    console.log("类组件绑定事件被触发了");
    console.log(`类组件中：${e.target},${str}`);
  }
  render() {
    return (
      <div
        style={{
          width: "100%",
          height: "60px",
          lineHeight: "60px",
          backgroundColor: "#00ffff",
          textAlign: "center",
        }}
        onClick={(e) => {
          this.clickHandler(e, "this is a msg");
        }}
      >
        点击触发类组件绑定事件
      </div>
    );
  }
}
```
## 三、组件状态
### 1. 类组件中数据驱动视图的基本写法
在`react hook`出来之前，函数组件是没有状态的，所以这里统一通过类组件来讲解。
现在，**类组件**已经不太常用了
+ 使用state，修改state通过setState实现，setState通过继承而来
+ this有指向问题，要当心。在指定onClick中的方法时，必须要用this.xxx，如果没有传，则为undefined
```javascript
class DivTemplate extends React.Component {
  state = {
    name: "state name",
  };
  clickHandler(e) {
    this.setState({
      name: "state name is changed",
    });
  }
  render() {
    return (
      <div
        className="button"
        onClick={(e) => {
          this.clickHandler(e);
        }}
      >
        {this.state.name}
      </div>
    );
  }
}
```
### 2. react的状态不可变
不要直接去修改state，修改数组和对象要重新使用setState去整体赋值

## 四、表单处理（input）
使用react处理表单，一般有两种方式
1. 受控组件
2. 非受控组件（不常用）

### 1. 受控表单组件
> 受控组件就是`input框可以被react组件状态控制`的组件，可以实现双向绑定

**双向绑定的步骤**
1. input绑定onChange或onInput事件
2. value指定某个变量
3. 事件在类中写好，然后在事件中使用setState去将input的value赋值给变量

```javascript
class DivTemplate extends React.Component {
  state = {
    message: "this is a push",
  };
  inputChange = (e) => {
    console.log("input changed!");
    this.setState({
      message: e.target.value,
    });
  };
  render() {
    return (
      <div>
        <input
          type="text"
          value={this.state.message}
          onInput={(e) => {
            this.inputChange(e);
          }}
        />
        <span>{this.state.message}</span>
      </div>
    );
  }
}
```
### 2. 非受控组件
> 组件的状态和表单控件状态不同步(只是表单控件的初值和组件的`state`一致)。没有双向绑定
使用createRef

```javascript
import React, { createRef } from "react";

class DivTemplate extends React.Component {
  msgRef = createRef();
  clickHandler(e) {
    console.log(this.msgRef.current.value);
  }
  render() {
    return (
      <div>
        <input type="text" className="input_border" ref={this.msgRef} />
        <div
          className="button"
          onClick={(e) => {
            this.clickHandler(e);
          }}
        >
          点击获取input的value内容
        </div>
      </div>
    );
  }
}
```
## 五、组件通信
### 1. 父子组件通信
函数子组件通过参数`props`，类子组件通过`this.props`，函数组件可以通过参数解构接收， v也可以通过props接收的。
父组件上定义传入子组件的变量

**`props`：**
**1. 只能单向，不能反过来修改。**
**2. 可以解构赋值**

如下，现在父组件的`state`种有一个`message`变量，想传入到子组件中 
父组件App.js：
```javascript
class App extends React.Component {
  state = {
    message: "this is message",
  };
  render() {
    return (
      <div>
        <SonC msg={this.state.message}></SonC>
        <SonF msg={this.state.message}></SonF>
      </div>
    );
  }
}
```
子组件SonF（函数组件）
```javascript
function SonF(props) {
  // props是一个对象，里面存着通过父组件传入的所有数据
  return (
    <div>
      函数子组件：<span>{props.msg}</span>
    </div>
  );
}
```
子组件SonC（类组件）
```javascript
class SonC extends React.Component {
  render() {
    return (
      <div>
        类子组件：<span>{this.props.msg}</span>
      </div>
    );
  }
}

```
### 2. 通过子组件传递给父组件
**原理：子组件调用父组件传递过来的函数，并且将想要传递的数据当成函数的实参**

不管是类组件或是函数组件都是以下步骤：
>1. 在父组件建立函数，传入子组件中
>2. 子组件用props接收父组件的函数，在函数中传递参数，然后在模板中执行函数
>3. 父组件形参接收子组件的传值即可。

以类组件举例：
```javascript
class Son extends React.Component {
  getSonMsg = this.props.getSonMsg;
  render() {
    return (
      <div>
        <div
          className="button"
          onClick={(str) => {
            this.getSonMsg("aaaaaaa");
          }}
        >
          点击
        </div>
      </div>
    );
  }
}
```
App.js中：
```javascript
class App extends React.Component {
  state = {
    messageFromSon: "",
  };
  getSonMsg = (str) => {
    this.setState({ messageFromSon: str });
  };
  render() {
    return (
      <div>
        <Son getSonMsg={this.getSonMsg}></Son>
        <span>从子组件中传过来的内容为：{this.state.messageFromSon}</span>
      </div>
    );
  }
}
```
### 3.兄弟组件通信
不想写了，反正就是两兄弟会通过父组件，进行数据的传递。一个子组件将数据传给父组件，父组件再将数据props给另一个子组件。
### 4.跨组件通信Context
父组件像任意的子孙组件传值，可以使用Context。
相当于vue的依赖传值。

步骤：
1. 新建一个组件，专门用来导出Provider和Consumer，然后让需要Provider和Consumer的组件引入该组件。不可以在需要的文件中直接用React.createContext去新建，因为在不同文件中生成的是不同实例。  

根组件App.js
```javascript
import Context from "./ContextComponent.js";

class App extends React.Component {
  state = {
    message: "this is message",
  };
  render() {
    let message = this.state.message;
    console.log(message);
    return (
      <Context.Provider value={message}>
        <div className="app">
          <SonA />
        </div>
      </Context.Provider>
    );
  }
}

```
需要引入的组件ContextComponent
```javascript
import "./App.css";
import React from "react";
import { createContext } from "react";

const Context = createContext();
const { Provider, Consumer } = Context;

export default { Provider, Consumer };
```
中间组件SonA的内容
```javascript
import "./App.css";
import React from "react";
import SonB from "./SonB.js";

function SonA() {
  return <SonB />;
}
export default SonA;
```

子孙组件内容
```javascript
import "./App.css";
import React from "react";
import Context from "./ContextComponent.js";

function SonB() {
  return <Context.Consumer>{(value) => <div>{value}</div>}</Context.Consumer>;
}

export default SonB;
```
## 六、组件进阶
### 1. children 属性（slot）
children存在于props里面，所以可以通过解构赋值，也可以通过props.children去获取使用。
表示该组件的子节点，只要组件内部有子节点，props中就有这个属性。
类型包括：
>1. 普通文本
>2. 普通标签
>3. jsx
>4. 函数

```javascript
function ListItem({ children }) {
  return (
    <div>
      ListItem:<div>{children}</div>
    </div>
  );
}
class App extends React.Component {
  render() {
    return <ListItem>this is child</ListItem>;
  }
}
```

### 2. props校验
有时候从父组件传过来的props，类型会不正确，比如子组件需要一个数组，而父组件传过来一个number，那么遍历就会出现问题。
这种情况下，就需要props校验。
相当于vue的

```javascript
props: [
  {
      str: String
      default () {
        return ''
      }
  }
]
```
支持类型：

>1. 基础类型（array、bool、func、number、object、string）
>2. react元素类型： element
>3. 必填项： isRequired，也就是传属性时不能缺少这个属性。
>4. 特定的结构对象：shape

**步骤：**
1. 安装`prop-types`
```javascript
yarn add prop-types
```
2. 在需要的组件中导入`prop-types`
```javascript
import propTypes from 'prop-types'
```
3. 对应`组件.propTypes = {}`设定规则
如下，表示名为`ListItem`的组件有一个lists被传进来，类型必须为`array`
```javascript
ListItem.propTypes = {
  lists: propTypes.array,
};
```
完整代码：
```javascript
import propTypes from "prop-types";
// 里面有各种校验规则

// eslint-disable-next-line

import React from "react";

ListItem.propTypes = {
  lists: propTypes.array,
};
```
```javascript
function ListItem({ children, lists = [] }) {
  return (
    <div>
      <div>
        ListItem:<div>{children}</div>
      </div>
      <div>
        {lists.map((list, index) => (
          <div key={list.id}>{list.name}</div>
        ))}
      </div>
    </div>
  );
}
```
```javascript
class App extends React.Component {
  state = {
    lists: [
      { id: 1, name: "sdfdsf" },
      { id: 2, name: "sdfdsf" },
      { id: 3, name: "sdfdsf" },
      { id: 4, name: "sdfdsf" },
    ],
    message: "sdfdsfsdfdsfegwegwe",
  };
  render() {
    return <ListItem lists={this.state.lists}>this is child</ListItem>;
  }
}
```
## 七、组件生命周期
只有类组件才有生命周期，因为类组件要实例化，而函数组件不需要。

![](https://upload-images.jianshu.io/upload_images/20892169-644b0a5e86b588e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 初始化阶段：

**1. 执行顺序：**
`constructor` --> `render` --> `componentDidMount`
**2. 功能：**
`constructor`：创建组件时最先执行，初始化时只执行一次，现在用的不多了。
`render`: 每次渲染时都会触发，所以不要在里面调用setState，有可能造成死循环。
`componetDidMount`：组件挂载（dom渲染完成）后执行，初始化的时候只执行一次。所以一般进行网络请求和dom操作。

#### 更新阶段：
**1. 执行顺序：**
`render` --> `componentDidUpdate`
**2. 功能：**
`render`：每次渲染都会触发
`componentDidUpdate`：组件更新后（dom渲染完成）触发。
两个钩子里面都不要调用setState()

#### 卸载阶段：
componentWillUnmount：组件卸载（从页面中消失）时触发，一般用来执行清理工作（比如：清理定时器等）

三个阶段，五個鉤子
```javascript
class App extends React.Component {
  constructor() {
    super();
    console.log("constructor");
  }
  componentDidMount() {
    console.log("mounted");
  }
  componentDidUpdate() {
    console.log("update");
  }
  componentWillUnmount() {
    console.log("unmount");
  }
  render() {
    return <div></div>;
  }
}
```

## 八、React Hooks
Hooks：使函數組件更強大，更靈活的鉤子
現在hooks暫時只能在函數組件中使用。因為它的作用就是為了讓hooks擁有狀態。
### 1. useState
useState提供給變量修改數據的方法。

步驟：
1.  從react中引入useState
2. 使用useState聲明變量，聲明修改變量的方法[變量，修改變量的方法]
3. 使用修改變量的方法修改數據
注意：不能直接修改原值，必須調用修改變量的方法，在下面的例子就是setCount
```javascript
import React from "react";
import { useState } from "react";
function App() {
  const [count, setCount] = useState(0);
  // 聲明變量，聲明修改變量的方法
  const clickHandler = () => {
    // 點擊時通過setCount修改count
    setCount(count + 1);
  };
  return (
    <div
      className="button"
      onClick={() => {
        clickHandler();
      }}
    >
      {count}
    </div>
  );
}
```
### 2. useState數據的讀取和修改
一句話：useState生成的初始值只在初次渲染生成，之後再次生成j都是用setstate修改得到的值！
注意事項：
1. 只能出現在函數組件中
2. 不能用於if/for等嵌套結構中，只能寫在函數組件最外層處。
### 3. useEffect 函數副作用
對於react組件來說，祝作用時根據數據渲染ui，那麼副作用就是除此之外的全部操作。

#### 常見的组件副作用
**1. 數據請求ajax發送**
**2. 手動修改dom**
**3. localStorage操作**
**4. 当watch用：**
useEffect有两个参数，第一个是函数，第二个参数是数组，用来存放变量，当数组存放变量发生改变时，第一个参数逻辑处理函数将会被执行（类似于监听）
vue的watch不一定是在dom渲染完畢之後執行，而useEffect一定是在dom渲染完畢後執行。
**5. 可以作为生命周期钩子使用，在每次render後執行（dom渲染後觸發）**
    + 没有第二个参数时,组件的初始话和更新都会执行 
    + 空数组时，初始化调用一次之后不再执行，相当于componentDidMount
    + 有一个值的数组时，该值有变化就执行
    + 有多个值的数组时，只要有一个值有变化就执行

```javascript
function App() {
  let [count, setCount] = useState(0);
  let clickHandler = () => {
    setCount(count + 1);
  };
  let [str, setStr] = useState("this is a message");

  useEffect(() => {
    document.title = count;
  }, [str]);

  let strClickHandler = () => {
    let tempStr = str;
    if (tempStr.length < 25) {
      tempStr = tempStr + "a";
      setStr(tempStr);
      setCount(count + 1);
    } else {
      console.log("標題不再更新");
    }
  };
  return (
    <div>
      <div
        className="button"
        onClick={() => {
          clickHandler();
        }}
      >
        点击增加count
      </div>
      <div
        className="button"
        onClick={() => {
          strClickHandler();
        }}
      >
        点击更改{str}，触发标题更新
      </div>
    </div>
  );
}
```
### 4. useRef 用於獲取真實dom的方法
>函數組件的useRef相當於類組件的createRef
綁定在元素上獲取該元素，綁定在組件上可以獲取該組件的所有屬性和方法。

步驟：
1. 引入`useRef`
2. 使用`useRef`新建一個變量，傳入參數為null，在其中有current可以獲取到組件信息或dom元素
3. 將該變量通過`ref`綁定到元素或組件上

假設現在有一個Test組件
```javascript
class Test extends React.Component {
  render() {
    return <div>aabbcc</div>;
  }
}
```
 需要在App組件中獲取Test組件的方法和屬性
```javascript
function App() {
  let testRef = useRef(null);
  let h1Ref = useRef(null);
  useEffect(() => {
    console.log("testRef", testRef.current);
    console.log("h1Ref", h1Ref.current);
  });
  return (
    <div>
      <Test ref={testRef}></Test>
      <h1 ref={h1Ref}>標題標題標題</h1>
    </div>
  );
}
```
testRef可以獲取到Test組件的方法與屬性，h1Ref可以獲取到h1這個dom元素。

對比類組件的createRef的用法
```javascript
class Test extends React.Component {
  state = {
    message: "this is a message",
  };
  divRef = createRef();
  clickHanlder() {
    console.log(this.divRef.current);
  }
  render() {
    return (
      <div
        className="button"
        ref={this.divRef}
        onClick={() => {
          this.clickHanlder();
        }}
      >
        aabbcc
      </div>
    );
  }
}
```

### 4. useContext 函数组件中的通信
在类组件中使用`createContext`进行后代组件通信。
函数组件中也需要使用`createContext`，两者在祖先组件中的写法一致。但函数组件中`Consumer`的部分需要变成使用`useContext`

```javascript
import React, { createContext, useContext, useState } from "react";
let Context = createContext();
```
```javascript
function SonA() {
  return <SonB></SonB>;
}
```
在SonB这个后代组件中，使用`useContext`去代替`Consumer`，就可以获取到`App`传过来的`count`数据
```javascript
function SonB() {
  let count = useContext(Context);
  return <h3 style={{ textAlign: "center" }}>{count}</h3>;
}
```
```javascript
function App() {
  let [count, SetCount] = useState(0);
  let addCount = () => {
    SetCount(count + 1);
  };
  return (
    <div>
      <Context.Provider value={count}>
        <SonA></SonA>
      </Context.Provider>
      <div
        className="button"
        onClick={() => {
          addCount();
        }}
      >
        从顶层组件像任意后代组件传递的count
      </div>
    </div>
  );
}
```
### 5. useCallback()
## 九、react-router
### 1. 安装router
```javascript
npm i react-router-dom
```
###1. BrowerRouter和HashRouter
**作用**：包裹整个应用，一个react应用只需要使用一次
**两种常用的router**: `HashRouter`和`BrowerRouter`
##### HashRouter相当于vue的`Hash`模式
地址域名后面会多一个#，比如
http://localhost:3000/#/first
##### BrowerRouter相当于vue的`History`（html5）模式
但这种模式需要后端配合，这个地址相当于前端为了好看生成的，不是真实的地址。如果直接访问某个二次跳转的链接，有可能404
http://localhost:3000/first
### 2. Link
用来指定导航链接
### 3. Routes
符合Link条件的试图会渲染到Routes里。
### 4. Route
用于指定导航链接，完成路由匹配。相当于vue的router-view
### 5. 编程式导航 - 跳转
***`react-router`***的跳转可以分为用***`Link`***跳转，和用***`useNavigate`***跳转两种。这里主要说***`useNavigate`***的用法。

**useNavigate直接跳转**：对应vue的\$router.push()，会在地址栈留下记录，可以回退。

**useNavigate添加` { replace: true } `参数**：对应vue的`\$router.replace()`，不会在地址栈中留下记录，无法回退到该页面，一般用于登录后的回退。

步骤：
1. 导入useNavigate
```javascript
import { useNavigate } from "react-router-dom";
```
2. 声明一个useNavigate方法
```javascript
let navigateLink = useNavigate();
```
3. 使用该方法，传入参数，第一个参数为地址，第二个参数为补充项，如上面说的是否要使用replace模式。
```javascript
navigateLink('/About',{replace:true})
```
完整例子：
```javascript
import { useNavigate } from "react-router-dom";
function Login() {
	let navigateLink = useNavigate();
	return (
		<div className="wrapper-index">
			<div
				className="button"
				onClick={() => {
					navigateLink("/about");
				}}>
				跳转到关于页
			</div>
		</div>
	);
}
```

### 6. 跳转传参 searchParams & params
##### searchParams 和 params的区别：
searchParams：地址为/index?name=xxx&id=xxx的形式，可以获取问号后的参数
params：地址为/index/111的形式，可以获取到111

#####步骤：
1. 首先引入useSearchParams或useParams
```javascript
import { useSearchParams, useParams } from "react-router-dom";
```
2. 初始化变量
```javascript
let [searchParams] = useSearchParams();
let params = useParams();
```
3. 通过变量获取参数
若为searchParams，则使用以下的的形式获取：
```javascript
{searchParams.get("id")}
```
若为params，则使用一下的形式获取：
```javascript
{params.id}
```
### 7. 二级路由
在route里面再配置route，然后在需要二级页面的地方导入outlet
App.js中
```javascript
function App() {
	return (
		<BrowserRouter>
			<Routes>
				<Route
					path="/"
					element={<Layout />}>
					<Route
						path="/article"
						element={<Article />}></Route>
					<Route
						path="/board"
						element={<Board />}></Route>
				</Route>
				<Route
					path="/login"
					element={<Login2 />}></Route>
			</Routes>
		</BrowserRouter>
	);
}
```
Layout.js中
```javascript
// 引入必要的内置组件
import { Outlet } from "react-router-dom";
function Layout() {
	return (
		<div>
			Layout
			<Outlet />
		</div>
	);
}

export default Layout;

```