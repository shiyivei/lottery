## 1 React基础

### 1.1 React 简介

1. 声明式UI（JSX）
2. 组件化：通过搭积木的方式拼成一个完整的页面，通过组件的抽象可以增加复用能力和提高可维护性
3. react既可以开发web应用，也可以使用同样的语法开发原生应用（react-native），比如安卓和IOS应用，甚至可以使用react开发VR应用，React更像一个源框架，为各种领域赋能

### 1.2 环境初始化

现在的前端开发基本都使用工程化的环境开始开发，所以我们先利用工具初始化一些文件，这个环节会装很多依赖包

```
npx create-react-app react-basic //创建react app（项目文件）
```

安装好依赖后打开文件

```
yarn start  //启动，基础的开发环境搭建完毕
```

### 1.3 熟悉目录

```
 "react": "^18.1.0",
 "react-dom": "^18.1.0", //两个react 核心包
```

 src就是写业务的文件夹，里面有多初始化的文件，我们删除只保留三个

```
 src % ls
App.js		index.css	index.js
```

修改两个js文件，只保留我们需要的内容

```
function App() {
  return (
    <div className="App">
      app
    </div>
  );
}

export default App;

```

```
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';


const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
//这里取消了严格模式
    <App />
);
```

## 2 JSX （JavaScript XML）

表示在JS代码中书写HTML结构

作用就是在React创建HTML结构

所写内容最终会被编译为浏览器能够识别的代码

### 2.1 表达式

```
//识别常规变量
//原生js方法调用
//三元运算符
//注意语句不能出现在{}中

const name = "shiyivei"

const getAge = () => {
  return 27
}

const flag = true
function App() {
  return (
    <div className="App">
      {name}
      {getAge()}
      {flag ? '真棒':'还需要努力'}
    </div>
  );
}

export default App;
```

### 2.2  列表渲染

```
//使用map重复渲染要返回的模版
//遍历列表时需要一个不可重复的key提高diff的性能
//key仅在内部使用，不会出现在真实的dom结构中

const chains = [
  {id: 1,name: 'polkadot'},
  {id: 2,name: 'ethereum'},
  {id: 3,name: 'solana'}
]

const flag = true
function App() {
  return (
    <div className="App">
     <ul>
       {chains.map(chains => <li key={chains.id}>{chains.name}</li>)}
     </ul>
    </div>
  );
}

export default App; 
```

### 2.3 条件渲染

```
//三元表达式，满足体条件才渲染一个span标签
//逻辑&&运算

const flag = true
function App() {
  return (
    <div className="App">
     <ul>
       {flag ? ( //可以使用一个小括号将要渲染的内容包裹起来详细处理
         <div> <span>this is span</span>
         </div>):null}
         
       { false && <span>this is span</span>}

     </ul>
    </div>
  );
}

export default App; 

//复杂逻辑的渲染
//建议收敛到一个函数中,然后在模版中直接调用

// const getHtag = (type) => {
//      if (type === 1) {
//        return <h1>this is span</h1>
//      }
//      if (type === 2) {
//        return <h2>this is span</h2>
//      }
//      if (type === 3) {
//        return <h3>this is span</h3>
//      }
//    }
   
//    const flag = true
//    function App() {
//      return (
//        <div className="App">
//         <ul>
//           {getHtag(1)}
//           {getHtag(2)}
//           {getHtag(3)}
//         </ul>
//        </div>
//      );
//    }
   
//    export default App;
```

### 2.4 JSX样式处理

```
import './app.css'
//行内样式 在元素身上绑定一个style属性即可

const style = {
  color: 'red',
  fontSize: '30px'
}

//类名样式，在元素身上绑定一个className属性即可,但要创建css样式用于存放样式文件，并且导入可以使用条件动态控制

function App() {
  return (
    <div className="App">
    <span style={style}>this is a span</span>
    <span className="active">test span</span>
    </div>
  );
}

export default App;
```

## 3 组件

### 3.1 函数组件

```
//定义函数组件

function HelloFn() {
  return <div>this is a component</div>;
}

//定义类组件

function App() {
  return (
    <div className="App">
      {/*渲染函数组件*/}
      <HelloFn />
      <HelloFn></HelloFn>
    </div>
  );
}

export default App;
```

### 3.2 类组件

```
import React from "react"; //先要引入React库
//类组件的创建和渲染

//使用继承的方式创建
class HelloComponent extends React.Component {
  render() {
    return <div>Class Component</div>;
  }
}

//渲染，方式同函数组件

function App() {
  return (
    <div>
      <HelloComponent />
      <HelloComponent></HelloComponent>
    </div>
  );
}

export default App;
```

### 3.3 事件绑定

```
绑定事件模版
on + 事件名称 = {Handler} Handler 是一个回调函数
```

```
import React from "react"; //先要引入React库
//定义函数组件

function Hello() {
  //函数组件中的handler
  const clickHandler = () => {
    console.log("the event in function component is triggered");
  };
  return <button onClick={clickHandler}>function component</button>;
}

//类组件的创建和渲染

//使用继承的方式创建
class HelloComponent extends React.Component {
  //类组件中的handler
  clickHandler = () => {
    console.log("the event in class component is triggered");
  };
  render() {
    //注意：定义时未使用const，使用时使用this，标准写法，指的是当前的实例对象
    return <button onClick={this.clickHandler}>Class Component</button>;
  }
}

//渲染，方式同函数组件

function App() {
  return (
    <div>
      {/*函数组件的渲染*/}
      <Hello />
      <Hello></Hello>
      {/*类组件的创建和渲染*/}
      <HelloComponent />
      <HelloComponent></HelloComponent>
    </div>
  );
}

export default App;
```

### 3.4 事件对象e

```
import React from "react"; //先要引入React库
//定义函数组件

function Hello() {
  //函数组件中的handler
  const clickHandler = (e) => {
    e.preventDefault();//使用事件对象e阻止跳转
    console.log("the event in function component is triggered", e);
  };
  return (
    <button onClick={clickHandler}>
      <a href="http://baidu.com">baidu</a>
    </button>
  );
}

//类组件的创建和渲染

//使用继承的方式创建
class HelloComponent extends React.Component {
  //类组件中的handler
  clickHandler = () => {
    console.log("the event in class component is triggered");
  };
  render() {
    //注意：定义时未使用const，使用时使用this，标准写法，指的是当前的实例对象
    return <button onClick={this.clickHandler}>Class Component</button>;
  }
}

//渲染，方式同函数组件

function App() {
  return (
    <div>
      {/*函数组件的渲染*/}
      <Hello />
      <Hello></Hello>
      {/*类组件的创建和渲染*/}
      <HelloComponent />
      <HelloComponent></HelloComponent>
    </div>
  );
}

export default App;
```

### 3.5 额外参数

可以只传递额外参数或者与事件对象e一起传递

```
function Hello() {
  //函数组件中的handler
  const clickHandler = (e, msg) => {
    //e.preventDefault(); //使用事件对象e阻止跳转
    console.log("the event in function component is triggered", e, msg);
  };
  return (
    <button onClick={(e) => clickHandler(e, "this is msg")}>
      <a href="http://baidu.com">baidu</a>
    </button>
  );
}
```

### 3.6 状态的定义和使用

步骤：初始化状态->读取状态->修改状态->影响视图

```
//组件状态

import React from "react";

class TestComponent extends React.Component {
  //定义组件状态
  state = {
    //此处可以定义组件的各种属性
    name: "shiyivei",
  };

  //定义函数修改名称,使用this调用内置的setSate改变函数状态
  changeName = () => {
    this.setState({ name: "qinjianquan" });
  };

  render() {
    return (
      <div>
        this is TestComponent, current name is {this.state.name},
        <button onClick={this.changeName}>changeName</button>
      </div>
    );
  }
}

// 根组件
function App() {
  return (
    <div>
      <TestComponent />
    </div>
  );
}

export default App;
```

编写组件就是编写原生js类或者函数

定义状态必须通过state，必须通过setState来设置，不能直接赋值

### 3.7 更改状态

不要直接修改状态值，而是更新状态值

```
import React from "react";

//定义类组件
class Counter extends React.Component {
  //类组件定义三步走，大写继承，render和return

  state = {
    counter: 0,
  };

  //定义修改handler
  changeCount = () => {
    this.setState({
      counter: this.state.counter + 1,
    });
  };

  render() {
    return (
      <button onClick={this.changeCount}>{this.state.counter}click</button>
    );
  }
}

function App() {
  return (
    <div>
      <Counter></Counter>
    </div>
  );
}

export default App;
```

### 3.8 React 的状态不可变

```
import React from "react";

//定义类组件
class Counter extends React.Component {
  //类组件定义三步走，大写继承，render和return

  state = {
    count: 0,
    list: [1, 2, 3], //数组
    person: {
      //对象
      name: "shiyivei",
      age: 27,
    },
  };

  //定义修改handler，所有的修改可以放到一个setState
  clickHandler = () => {
    this.setState({
      count: this.state.count + 1,
    });

    // this.setState({
    //   list: [...this.state.list, 4], //改变数组
    // });
    //删除操作
    this.setState({
      list: this.state.list.filter((item) => item !== 2),
    });

    this.setState({
      //改变对象
      person: {
        ...this.state.person, //展开字符先展开
        name: "qinjianquan",
      },
    });
  };

  render() {
    return (
      <>
        <ul>
          {this.state.list.map(
            (
              item //先遍历
            ) => (
              <li key={item}>{item}</li>
            )
          )}
        </ul>

        <div>{this.state.person.name}</div>

        <div>{this.state.count}</div>

        <div>
          <button onClick={this.clickHandler}>changeState</button>
        </div>
      </>
    );
  }
}

function App() {
  return (
    <div>
      <Counter />
    </div>
  );
}

export default App;

```

###  3.9 表单处理

#### 3.9.1 受控组件

```
import React from "react";

//定义类组件
class Counter extends React.Component {
  //1.声明一个React的状态
  state = {
    message: "this is message",
  };

  //回调函数
  inputChange = (e) => {
    console.log("change 事件触发了", e);
    //4.拿到输入的值，变更声明值的状态
    this.setState({
      message: e.target.value,
    });
  };

  render() {
    //2.定义文本输入框，值绑定为声明好的状态值（其实就是给输出框绑定一个初始值）
    //3.给input框绑定一个change事件，就可以拿到当前输入框输入的值
    return (
      <input
        type="text"
        value={this.state.message}
        onChange={this.inputChange}
      />
    );
  }
}

function App() {
  return (
    <div>
      <Counter />
    </div>
  );
}

export default App;

```

#### 3.9.2 非受控组件

```
import React, { createRef } from "react"; //从react中导入函数

//定义类组件
class Counter extends React.Component {
  //直接使用函数初始化实例
  msgRef = createRef("msg");

  //handler

  getValue = (value) => {
    //值在current里面存储
    console.log(this.msgRef.current.value);
  };

  render() {
    return (
      <>
        <input type="text" ref={this.msgRef} />
        //直接能够获取输入的值
        <button onClick={this.getValue}>点击获取输入框的值</button>
      </>
    );
  }
}

function App() {
  return (
    <div>
      <Counter />
    </div>
  );
}

export default App;

```

## 4 组件通信

父子关系/兄弟关系/其他关系

### 4.1 父传子

```
import React from "react";

//define a function component
function SonFunc(props) {
  //receive the value from its father component
  return <div>this is func component,{props.message}</div>;
}

//define a class component
class SonClass extends React.Component {
  //the same as above
  render() {
    return <div>this is class component,{this.props.message}</div>;
  }
}


class App extends React.Component {

  //1.define a variable in state and pass it in return value
  state = {
    message: "Hello",
  };

  render() {
    return (
      <>
        <SonFunc msg={this.state.message} />
        <SonClass msg={this.state.message} />
      </>
    );
  }
}

export default App;
```

关于Pros，只读，支持传递任何类型数据，包括函数和JSX（可以把它理解为插槽）

```
import React from "react";

//define a function component
// function SonFunc(props) {
//   console.log(props);
//   //receive the value from its father component

//   //destructuring assignment
//   const { list, userInfo, getMes, child } = props;
//   return (
//     <div>
//       this is func component,
//       {list.map((item) => (
//         <p key={item}>{item}</p>
//       ))}
//       {userInfo.name}
//       <button onClick={getMes}>触发父组件传入的函数</button>
//       {child}
//     </div>
//   );
// }

//destructuring assignment
function SonFunc({ list, userInfo, getMes, child }) {
  //receive the value from its father component
  return (
    <div>
      this is func component,
      {list.map((item) => (
        <p key={item}>{item}</p>
      ))}
      {userInfo.name}
      <button onClick={getMes}>触发父组件传入的函数</button>
      {child}
    </div>
  );
}

//define a class component
class SonClass extends React.Component {
  //the same as above
  render() {
    return <div>this is class component,{this.props.list}</div>;
  }
}

class App extends React.Component {
  //1.define a variable in state and pass it in return value
  state = {
    list: ["Hello", "thanks", "are", "you", "ok"],
    userInfo: {
      id: 1,
      name: "john",
    },
  };

  getMes = () => {
    console.log("the msg in father component");
  };

  render() {
    return (
      <>
        <SonFunc
          list={this.state.list}
          userInfo={this.state.userInfo}
          getMes={this.getMes}
          child={<span>this is span</span>}
        />
        <SonClass list={this.state.list} />
      </>
    );
  }
}

export default App;
```

### 4.2 子传父

调用父组件传递过来的函数，并且把想要传递的数据当成函数的实参

```
import React from "react";

function SonFunc(props) {
  //receive the value from its father component
  //call func and pass a value to its father,use format of Parentheses
  const { getSonMsg } = props;
  function callHandler() {
    getSonMsg("the data comes from son component");
  }
  return (
    <div>
      this is son component
      <button onClick={callHandler}>click</button>
    </div>
  );
}

class App extends React.Component {
  //1.define a variable in state and pass it in return value
  state = {
    list: ["Hello", "thanks", "are", "you", "ok"],
  };

  //define a func and pass it to son
  getSonMsg = (sonMsg) => {
    console.log(sonMsg);
  };

  render() {
    return (
      <>
        <SonFunc getSonMsg={this.getSonMsg} />
      </>
    );
  }
}

export default App;
```

### 4.3 兄弟组件间传递参数

子-父-子

```
import React from "react";

function SonCA(props) {
  return <div>this is A,{props.sendAMsg}</div>;
}

function SonCB(props) {
  const msg = "it comes from B";
  return (
    <div>
      this is B<button onClick={() => props.getMsg(msg)}>发送数据</button>
    </div>
  );
}

class App extends React.Component {
  state = {
    sendAMsg: "test",
  };

  getMsg = (msg) => {
    console.log(msg);

    this.setState({
      sendAMsg: msg,
    });
  };

  render() {
    return (
      <>
        <SonCA sendAMsg={this.state.sendAMsg} />
        <SonCB getMsg={this.getMsg} />
      </>
    );
  }
}

export default App;

```

### 4.4 跨组件传递数据

```
import React, { createContext } from "react"; //1

const { Provider, Consumer } = createContext(); //2

// Fixed usage:
{/* <Consumer>{(value) => <span>{value}</span>}</Consumer>;
 <Provider value={this.state.msg}>
 <ComA />
</Provider> */}

function ComA() {
  return (
    <div>
      this is A
      <ComC />
    </div>
  );
}

function ComC() {
  return (
    //4
    <div>
      this is C<Consumer>{(value) => <span>{value}</span>}</Consumer>
    </div>
  );
}

class App extends React.Component {
  state = {
    msg: "this is message",
  };
  //3
  render() {
    return (
      <Provider value={this.state.msg}>
        <ComA />
      </Provider>
    );
  }
}

export default App;
```

父传子子传父案例

```
import React, { createContext } from "react"; //1

//render list

function ListItem({ item, delItem }) {
  //son pass value to father and reverse

  return (
    <>
      <h3>{item.title}</h3>
      <p>{item.price}</p>
      <p>{item.desc}</p>
      <button onClick={() => delItem(item.id)}>delete</button>
    </>
  );
}

//provider data
//component may be construct after finishing all tasks logic
class App extends React.Component {
  state = {
    list: [
      { id: 1, title: "sugar", price: 18.8, info: "big discount" },
      { id: 2, title: "bread", price: 10.8, info: "big discount" },
      { id: 3, title: "fruit", price: 30.8, info: "big discount" },
    ],
  };
  //

  delItem = (id) => {
    this.setState({ list: this.state.list.filter((item) => item.id !== id) });
  };

  render() {
    return (
      <div>
        {this.state.list.map((item) => (
          <ListItem key={item.id} item={item} delItem={this.delItem} />
        ))}
      </div>
    );
  }
}

export default App;
```

## 5 组件进阶

### 5.1 children 属性

当在组件中插入其他内容时，会自动赋值给children，可以是文本、标签元素、函数和JXS

### 5.2 Props类型校验基础使用

为了增加组件的健壮性

校验不是内置的得安装一个属性校验包

```
yarn add prop-types
```

```
import React from "react";
import PropTypes from "prop-types"; //package includes some rules to verify types

function Test({ list }) {
  return (
    <div>
      {list.map((item) => (
        <p>{item}</p>
      ))}
    </div>
  );
}

//add rules
Test.propTypes = {
  list: PropTypes.array,
};

class App extends React.Component {
  render() {
    return (
      <div>
        <Test list={[1, 2, 3]} />
      </div>
    );
  }
}

export default App;
```

四种常见结构

常见类型 + element + isRequired + 特定的结构对象：shape({})

```
import React from "react";
import PropTypes from "prop-types"; //package includes some rules to verify types

function Test({ list }) {
  return <div>this is test</div>;
}

//add rules
Test.propTypes = {
  list: PropTypes.array.isRequired,
};

class App extends React.Component {
  render() {
    return <Test />;
  }
}

export default App;
```

### 5.3 函数组件props默认值

```
import React from "react";
import PropTypes from "prop-types"; //package includes some rules to verify types

//recommends
function Test({ pageSize = 10 }) {
  return <div>this is test{pageSize}</div>;
}

//add rules
Test.propTypes = {
  list: PropTypes.array.isRequired,
};
//deprecated
// Test.defaultProps = {
//   pageSize: 10,
// };
class App extends React.Component {
  render() {
    return <Test pageSize={20} />;
  }
}

export default App;

```

### 5.4 类组件的props默认值

```
import React from "react";
import PropTypes from "prop-types"; //package includes some rules to verify types

//recommends
class Test extends React.Component {
  //second method, recommend
  static defaultProps = {
    pageSize: 10,
  };
  render() {
    return <div>{this.props.pageSize}</div>;
  }
}

//first method
// Test.defaultProps = {
//   pageSize: 10,
// };

class App extends React.Component {
  render() {
    return <Test pageSize={20} />;
  }
}

export default App;
```

### 5.5 组件的生命周期

只有类组件才有生命周期

render在每次中都会触发，只要引起视图变化，就会触发render，所以不要在render中调用setState()

```
import React from "react";
import PropTypes from "prop-types"; //package includes some rules to verify types

//recommends
class Test extends React.Component {}

class App extends React.Component {
  constructor() {
    super();
    console.log("constructor");
  }
  componentDidMount() {
    console.log("componentDidMount");
  }

  state = {
    count: 0,
  };

  clickHandler = () => {
    this.setState({
      count: this.state.count + 1,
    });
  };
  render() {
    console.log("render");
    return (
      <div>
        this is div
        <button onClick={this.clickHandler}>{this.state.count}click</button>
      </div>
    );
  }
}

export default App;
```

更新阶段和渲染阶段

```
 //will execute with render
  componentDidUpdate() {
    console.log("componentDidUpdate");
  }
  render() {
    console.log("render");
    return (
      <div>
        this is div
        <button onClick={this.clickHandler}>{this.state.count}click</button>
      </div>
    );
  }
```

componentWillUnmount 清理定时器

## 6 Hooks

使函数组件更强大，更灵活的钩子

### 6.1 基础知识

基本使用

```
import { useState } from "react";

function App() {
  //provide a value and a method
  const [count, setCount] = useState(0);
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  );
}

export default App;
```

### 6.2 状态的读取和修改

解构成对出现和使用，不能调换顺序

### 6.3 组件的更新过程

首次渲染和更新渲染

### 6.4 useState 使用规则说明

可以重复使用，每次相互独立，不允许在条件语句中执行

```
import { useState } from "react";

function App() {
  //provide a value and a method
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(true);
  const [list, setList] = useState([]);

  console.log(count);

  function Test() {
    setCount(count + 1);
    setFlag(true);
    setList([1, 2, 3, 4]);
  }

  return (
    <div>
      <div>count:{count}</div>
      <div>flag:{flag ? 1 : 0}</div>
      <div>list:{list.join("-")}</div>
      <button onClick={Test}>change</button>
    </div>
  );
}

export default App;
```

### 6.5 副作用和基础使用

副作用其实就是修改函数外部的变量状态

```
import { useState, useEffect } from "react";

function App() {
  //provide a value and a method
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = count;
  });

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  );
}

export default App;
```

### 6.6 通过依赖项控制useEffect执行时机

1. 默认情况下，一上来先执行一次，接着组件更新一次就执行一次
2. 添加空数组依赖项，只会在初始化时执行一次

```
 useEffect(() => {
    console.log("The side effect is executed again");
    document.title = 1;
  }, []); 
```

3. 依赖特定项时，组件初始化时执行一次，依赖的特定项发生变化时会再次执行

   注意：如果在回调函数中使用了某个数据状态，就应该把它加入到依赖项中

```
useEffect(() => {
    console.log("The side effect is executed again");
    document.title = count;
    console.log(name);
  }, [count,name]);
```

某种意义上，hooks的出现就是不想哟用生命周期的概念也可以写业务代码

### 6.7 使用案例

```
import { useState } from "react";

function useWindowScroll () {
  
  const [y,setY] = useState(0) //返回一个值和值的方法

  window.addEventListener('scroll',() => {
    const h = document.documentElement.scrollTop;//获取变动值
    setY(h) //赋值给y
  })
  return [y]
}
```

```
import { useWindowScroll } from "./hooks/useWindowScroll";

function App() {
  const [y] = useWindowScroll(); //直接调用了属于是
  console.log(y);
  return <div style={{ height: "12000px" }}>{y}</div>;
}

export default App;
```

将新改变的值同步至本地

```
import { useEffect, useState } from "react";

export function useLocalStorage(key, defaultValue) {
  const [messages, setMessages] = useState(defaultValue);

  useEffect(() => {
    window.localStorage.setItem(key, messages);
  }, [messages, key]);

  return [messages, setMessages];
}
```

```
import { useWindowScroll } from "./hooks/useWindowScroll";
import { useLocalStorage } from "./hooks/useLocalStorage";

function App() {
  const [y] = useWindowScroll();
  const [message, setMessage] = useLocalStorage("hook-key", "阿菲");

  setTimeout(() => {
    setMessage("yivei");
  }, 5000);

  return (
    <div style={{ height: "12000px" }}>
      {y} {message}
    </div>
  );
}

export default App;
```

### 6.8 回调函数的参数

```
import React from "react";
import { useState, useEffect } from "react";

function Counter(props) {
  const [count, setCount] = useState(() => {
    return props.count + 1;
  });
  console.log(count);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
function App() {
  return (
    <div>
      <Counter count={10} />
      <Counter count={20} />
    </div>
  );
}

export default App;
```

```
import React from "react";
import { useState, useEffect } from "react";

function getDefaultValue() {
  for (let i = 0; i < 10000; i++) {}
  return "1";
}

function Counter(props) {
  const [count, setCount] = useState(() => {
    // return props.count + 1;
    return getDefaultValue();
  });
  console.log(count);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
function App() {
  return (
    <div>
      <Counter />
      <Counter count={10} />
      <Counter count={20} />
    </div>
  );
}

export default App;
```

### 6.9 清除副作用

```
import React from "react";
import { useState, useEffect } from "react";

function Test() {
  useEffect(() => {
    let timer = setInterval(() => {
      console.log("timer is executed");
    }, 1000);

    return () => {
      clearInterval(timer);
    };
  }, []);
  return <div>this is test</div>;
}
function App() {
  const [flag, setFlag] = useState(true);
  return (
    <div>
      {flag ? <Test /> : null}
      <button onClick={() => setFlag(!flag)}>switch</button>
    </div>
  );
}

export default App;
```

### 6.10 useEffect 发送网络请求

```
import React from "react";
import { useState, useEffect } from "react";

function App() {
  useEffect(() => {
    async function loadData() {
      //fetch is method to get info which browser support natively
      const res = await fetch("http://geek.itheima.net/v1_0/channels")
        .then((response) => response.json())
        .then((data) => console.log(data));
    }
    loadData();
  }, []);

  return <div>loadData()</div>;
}

export default App;
```

### 6.11 useRef

```
import React from "react";
import { useRef, useEffect } from "react";
class TestC extends React.Component {
  getName = () => {
    return "this is child test";
  };

  render() {
    return <div>this is class component</div>;
  }
}

function App() {
  const testRef = useRef(null);
  const h1Ref = useRef(null);

  useEffect(() => {
    console.log(testRef, h1Ref);
  });

  return (
    <div>
      <TestC ref={testRef} />
      <h1 ref={h1Ref}>this is h1</h1>
    </div>
  );
}

export default App;
```

useEffect的回调是在dom渲染之后

### 6.12 useContext 使用

```
import React from "react";
import { useContext, useState, createContext } from "react";

const Context = createContext();

function ComA() {
  const count = useContext(Context);
  return (
    <div>
      it comes from A
      <br />
      App passed data is: {count}
      <ComC />
    </div>
  );
}

function ComC() {
  // const count = useContext(Context);
  const count = useContext(Context);
  return (
    <div>
      it comes from C
      <br />
      App passed data is: {count}
    </div>
  );
}

function App() {
  const [count, setCount] = useState(100);
  return (
    <div>
      <Context.Provider value={count}>
        <ComA />
        <button
          onClick={() => {
            setCount(count + 1);
          }}
        >
          +
        </button>
      </Context.Provider>
    </div>
  );
}

export default App;

```

## 7 Router

### 7.1 路由的基础使用

```
import Home from "./Home";
import About from "./About";
import { BrowserRouter, Link, Routes, Route } from "react-router-dom";

function App() {
  return (
    //non-hash model router
    <BrowserRouter>
      {/*specify the link to switch*/}
      <Link to="/">Home</Link>
      <Link to="/About">About</Link>

      <Routes>
        {/*the relationship of component and path*/}
        <Route path="/" element={<Home />} />
        <Route path="/About" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;

```

### 7.2 路由模式组件说明

两种：HashRouter, BrowserRouter

### 7.3 编程式导航-实现跳转

```
import { useNavigate } from "react-router-dom"; //1.import function

function Login() {
  const navigate = useNavigate(); //define a function

  //use a new function to use function above
  function goAbout() {
    navigate("/About", { replace: true });
  }

  return (
    <div>
      Login
      <button onClick={goAbout}>go to About</button>
    </div>
  );
}

export default Login;

```

### 7.4 编程式导航-跳转携带参数

```
import { useSearchParams } from "react-router-dom";
function About() {
  const [params] = useSearchParams();
  //params is an object with a method get
  const id = params.get("id");
  const name = params.get("name");
  return (
    <div>
      The id is {id}, the name is {name}
    </div>
  );
}

export default About;
```

方式二

```
import Home from "./Home";
import About from "./About";
import Login from "./Login";

import {
  HashRouter,
  BrowserRouter,
  Link,
  Routes,
  Route,
} from "react-router-dom";

function App() {
  return (
    //non-hash model router
    <BrowserRouter>
      {/*specify the link to switch*/}
      <Link to="/">Home</Link>
      <Link to="/About">About</Link>

      <Routes>
        {/*the relationship of component and path*/}
        <Route path="/" element={<Home />} />
        <Route path="/About/:id" element={<About />} />
        <Route path="/Login" element={<Login />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;

```

### 7.5 嵌套路由的实现

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Layout from "./Layout";
import Login from "./Login";
import Board from "./Board";
import Article from "./Article";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route path="board" element={<Board />}></Route>
          <Route path="article" element={<Article />}></Route>
        </Route>
        <Route path="/login" element={<Login />}></Route>
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

默认路由

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Layout from "./Layout";
import Login from "./Login";
import Board from "./Board";
import Article from "./Article";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Board />}></Route>
          <Route path="board" element={<Board />}></Route>
          <Route path="article" element={<Article />}></Route>
        </Route>
        <Route path="/login" element={<Login />}></Route>
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

404 页面

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Layout from "./Layout";
import Login from "./Login";
import Board from "./Board";
import Article from "./Article";
import NotFound from "./NotFound";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Board />}></Route>
          <Route path="board" element={<Board />}></Route>
          <Route path="article" element={<Article />}></Route>
        </Route>
        <Route path="/login" element={<Login />}></Route>
        <Route path="*" element={<NotFound />}></Route>
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

## 8 Mobx

优势：简单，最优渲染，架构自由

### 8.1 环境搭建

```
npx create-react-app react-mob //react项目
```

```
yarn add mobx mobx-react-lite
```

### 8.2 创建store

```
import { makeAutoObservable } from "mobx";

class CounterStore {
  count = 0; //define a variable

  constructor() {
    makeAutoObservable(this); //make it responsive
  }

  addCount = () => {
    //change data
    this.count++;
  };
}

const counterStore = new CounterStore();

export { counterStore };
```

```
import { counterStore } from "./store/counter";
import { observer } from "mobx-react-lite";

function App() {
  return (
    <div className="App">
      {counterStore.count}
      <button onClick={counterStore.addCount}>+</button>
    </div>
  );
}

export default observer(App);
```

### 8.3 mobx-computed 计算属性

```
import { makeAutoObservable } from "mobx";

class CounterStore {
  count = 0; //define a variable
  list = [1, 2, 3, 4, 5, 6];

  constructor() {
    makeAutoObservable(this); //make it responsive
  }

  addCount = () => {
    //change data
    this.count++;
  };

  get filterList() {
    return this.list.filter((item) => item > 2);
  }
  addList = () => {
    return this.list.push(7, 8, 9);
  };
}

const counterStore = new CounterStore();

export { counterStore };
```

```
import { counterStore } from "./store/counter";
import { observer } from "mobx-react-lite";

function App() {
  return (
    <div className="App">
      {counterStore.count}
      {counterStore.filterList.join("-")}
      <button onClick={counterStore.addCount}>+</button>
      <button onClick={counterStore.addList}>changeList</button>
    </div>
  );
}

export default observer(App);
```

### 8.4 模块化

```
import { ListStore } from "./list.Store";
import { CounterStore } from "./counter.Store";
import React, { createContext } from "react";

class RootStore {
  constructor() {
    this.CounterStore = new CounterStore();
    this.ListStore = new ListStore();
  }
}

const rootStore = new RootStore();
const context = React.createContext(rootStore);
const useStore = () => React.useContext(context);

export { useStore };
```

```
import { observer } from "mobx-react-lite";
import { useStore } from "./store/index";

function App() {
  const rootStore = useStore();
  console.log(rootStore);
  return (
    <div className="App">
      {rootStore.CounterStore.count}
      <button onClick={rootStore.CounterStore.addCount}>+</button>
    </div>
  );
}

export default observer(App);
```