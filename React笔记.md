## 一. 基础

1. 在html中使用
   
   + 文件引入顺序
     
     ```html
     <!-- 1. 引入核心库 -->
     <script src="../js/react.development.js"></script>
     
     <!-- 2. 引入扩展库 -->
     <script src="../js/react-dom.development.js"></script>
     
     <!-- 3. 引入babel库 -->
     <script src="../js/babel.min.js"></script>
     ```
   
   + 脚本类型: <font color="orange">text/bable</font>
     
     ```html
     <script type="text/babel"></script> <!-- 为了写 jsx -->
     ```
   
   + 简单使用
     
     ```jsx
     // 1. 单行 (注意不要加引号)
     const VDOM = <h1>Hello React</h1>
     
     // 2. 多行
     const VDOM = (
         <h1>
             <span>Hello React</span>
         </h1>
     )
     
     ReactDOM.render(VDOM, document.querySelector('root');
     ```

2. jsx语法规则
   
   + 插值语法
     
     ```jsx
     // 使用一对花括号来包裹 js定义的变量
     // !!! 注意只是js表达式, 不是js语句(代码(if, for...)
     const text = "Hello React";
     
     const VDOM = (
         <h1>
             <span>{text}</span>
         </h1>
     )
     ```
   
   + 类名
     
     ```jsx
     // 为了规避 关键字class
     // 将html的class改成了
     const className = "title";
     
     const VDOM = (
        <h1 className={className}>
            Hello
        </h1>
     )
     ```
   
   + html标签的style
     
     ```jsx
     // 1. 要用 双花括号{{ key: value}} 包裹 style 属性值 
     // 2. 如果 css 属性包含 连字符 '-' 的, 要使用小驼峰命名法
     // 3. attr 的值, 必须是字符串形式(在这里就仅指 '' 这种),否则会当作变量来识别
     
     const style = {
          backgroundColor: '#ccc',
          textAlign: 'center',
      }
     
     const VDOM = (
         <h1 style={{ backgroundColor: '#ccc'}}> </h1>
         // 或者
         <h1 style={style}> </h1>
     )
     ```
   
   + DOM只能有一个根标签(类似Vue2)
   
   + 每个标签都必须要有闭合标签(主要是input要注意, 自结束即可)
   
   + arr/obj 入门遍历
     
     ```jsx
     /*
     *    1. 必须是有返回值的(即js表达式)
     *    2. 需要用一对花括号来包裹
     */
     const list = ["Angular", "React", "Vue"];
     
     <ul>
         {
             list.map((item, index) => {
                 return <li key={index}>{item}</li>
             })
         }
     </ul>
     ```

3. 组件化
   
   + 函数式组件（简单组件）
     
     ```jsx
     function Header() {
          const text = "我是Header组件";
          return (
              <header>{text}</header>
          )
      }
     
     ReactDOM.render(
         <MyComponent />, 
         document.querySelector('#app')
     );
     
     ```
   
   + 类式组件（复杂组件）
     
     ```jsx
     class MyComponent extends React.Component {
         render() {
             return <div>Hello World!</div>
         }
     }
     
     ReactDOM.render(
         <MyComponent />, 
         document.querySelector('#app')
     );
     
     ```

4. <font color="orange">state</font>
   
   + 简单使用(可以忽略这点, 看下面简写的)
     
     ```jsx
     class Weather extends React.Component {
         constructor(props) {
             super(props);
     
             this.state = {
                 isHot: true,
             }
         }
     
         render() {
             const { isHot } = this.state;
             return <h1>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
         }
     }
     
     ReactDOM.render(
         <Weather />,
         document.querySelector('#app')
     )
     ```
   
   + (可以忽略这点, 看下面简写的)事件绑定<font color="red">(注: 无法直接修改state内的属性)</font>
     
     ```jsx
     class Weather extends React.Component {
        constructor(props) {
            super(props);
     
            this.state = {
                isHot: true,
     
        }
            // !!! 函数 this 修正 !!!
            this.trans = this.trans.bind(this);
        }
     
        render() {
            const { isHot } = this.state;
     
            return (
                <h1 onClick={this.trans}>
                    今天天气很{isHot ? '炎热' : '凉爽'}
                </h1>
            )
        }
     
        trans() {
            let { isHot } = this.state;
            // 注意只能通过 setState 来修改 state 内属性
            // 否则, 虽然能 log 出修改成功的值, 但是本次修改不被 React 认可
            this.setState({ isHot: !isHot });
        }
     }
     
     ReactDOM.render(
        <Weather />,
        document.querySelector('#app')
     )
     ```
   
   + 简写方式
     
     ```jsx
     // 简写的核心: 类内(非构造器)赋值, 所以甚至不需要去 super() 构造器
     class Weather extends React.Component {
        // 1. state 可直接在类里维护, 也可以在 构造器 里
        // 注意类内 维护 state 是直接赋值, 不用加 this
        state = {
            isHot: true,
        }
     
        // 2. 函数使用 赋值+ 箭头函数
        changeWeather = () => {
            const { isHot } = this.state
            this.setState({ isHot: !isHot });
        }
     
        render() {
            const { isHot } = this.state;
            return (
                <h1 onClick={this.changeWeather}>
                    今天天气很{isHot ? '炎热' : '凉爽'}
                </h1>
            )
        }
     }
     ```

5. <font color="orange">props</font> 
   
   + 简单使用
     
     ```jsx
     // 核心: 类似 html 的 attr 方式传值, 在类里面用 this.props 接收即可
     class Person extends React.Component {
     
        render() {
            const { name, sex, age } = this.props;
     
            return (
                <section>
                    <ul>
                        <li>name: {name}</li>
                        <li>sex: {sex}</li>
                        <li>age: {age}</li>
                    </ul>
                    <hr />
                </section>
            )
        }
     }
     
     // 1. 直接传值
     ReactDOM.render(
        <Person name="Lumina" sex="male" age="18" />,
        document.querySelector('#app')
     )
     
     // 2. 展开
     const obj = {
        name: "Lumina",
        sex: "male",
        age: 18,
     }
     ReactDOM.render(
        <Person {...obj} />,
        document.querySelector('#app1')
     )
     ```
   
   + 类型限制
     
     ```jsx
     // 因为没有该script, 所以只做简易版, 写在类里面
     
     [static] propType = {
        要限制的props-key: PropType.string, // 限制为string格式, 但错了也渲染
        要限制的props-key: PropType.number, // 限制为string格式, 但错了也渲染
        要限制的props-key: PropType.func, // 限制为string格式, 但错了也渲染
     
        要限制的props-key: PropType.string.isRequired, // 限制为string格式, 必填项
     }
     
     [static] defaultProps = {
        需要有默认值的props-key: "", // 不填, 则显示该默认值
     }
     ```
   
   + 知识补充: <font color="orange">展开运算符(...)</font>
     
     ```jsx
     // 1. 数组展开
     const arr = [1, 2, 4, 6, 8];
     console.log(...arr);    //  res: 1, 2, 4, 6, 8(即展开数组)
     
     // 2. 函数参数展开
     function add(...nums) {
        return nums.reduce((pre, cur) => {
            return pre + cur;
        })
     }
     add(1, 2, 3, 4, 7);    // res: 17, 即展开后, 逐个进行了累加
     
     // 3. 对象展开(注意是不能直接展开的, 要加上{})
     const obj = {
        a: 1
     };
     console.log({ ...obj});    // res: { a: 1 }
     
     // 4. 对象浅拷贝
     const obj = { a: 1 };
     const obj2 = {...obj};
     
     // 5. 对象浅拷贝, 同时合并新的k-v, 若本身拥有, 则以最新的为准
     const obj = { a: 1 };
     const obj1 = { ...obj, name: "Alice" }
     console.log(obj1);    // res: {a: 1, name: 'Alice'}
     ```

6. <font color="orange">refs</font>(不要过度使用)<font color="red">(反正要使用this.refs的方法都已经弃用, 直接使用createRef()</font>
   
   + 字符串形式ref<font color="red">(注: 可能在未来废弃, 所以不推荐使用)</font>
     
     ```jsx
     class InputUse extends React.Component {
         showData = () => {
             console.log("showData", this.refs.showData.value);
         }
     
         render() {
             return (
                 <section>
                     <input ref="showData" type="text" />
                     <input type="button" value="点击提示左侧数据" onClick={this.showData} />
                 </section>
             )
         }
     }
     
     ReactDOM.render(<InputUse />, document.querySelector('#app'))
     ```
   
   + 回调形式ref
     
     ```jsx
     // 核心: ref={n => this.node = n}
     // 初始化时, 会进行两次调用
     //     (1) 传参为 null
     //     (2) 传参为 DOM元素
     // 解决方法: 将该回调, 在类中定义, 然后在html直接调用该函数即可
     
     class InputUse extends React.Component {
         showData = () => {
             console.log("showData", this.dataNode.value);
         }
     
         saveInput = (n) => {
             this.dataNode = n;
         }
     
         render() {
             return (
                 <section>
                     { // 方式一 }
                     <input
                         type="text"
                         ref={n => this.saveInput}
                         onKeyUp={this.showData}
                     />
                     { // 方式二 }
                     <input
                         type="text"
                         ref={n => this.dataNode = n}
                         onKeyUp={this.showData}
                     />
     
                     <input
                         type="button"
                         value="点击提示左侧数据"
                         onClick={this.showData}
                     />
                 </section>
             )
         }
     }
     
     ReactDOM.render(<InputUse />, document.querySelector('#app'))
     ```
   
   + createRef<font color="orange">(注: 繁琐, 但官方推荐, 很像内联回调形式)</font>
     
     ```jsx
           // 核心: React.createRef()
      // 一个 React.createRef() 只能用于 一个节点
      class InputUse extends React.Component {
          showData = () => {
              console.log("showData", this.inputRef.current.value);
          }
     
          blurData = () => {
              console.log("blurData", this.blurData.current.value);
          }
     
          inputRef = React.createRef();
          blurRef = React.createRef();
     
          render() {
              return (
                  <section>
                      <input
                          type="text"
                          ref={this.inputRef}
                      />
                      <button onClick={this.showData} >点我提示左侧数据</button>
                      <input
                          type="text"
                          ref={this.blurRef}
                          onKeyUp={this.showData}
                      />
                  </section>
              )
          }
      }
     
      ReactDOM.render(<InputUse />, document.querySelector('#app'))
     ```

7. 注意: React中的柯里化的使用不是在render函数里面, 而是在自己定义的函数里面
   
   ```jsx
   // 两种都推荐, 也都可以用
   class InputUse extends React.Component {
       state = {
           username: "",
           psd: "",
       }
   
       login = (dataType) => {
           return (e) => {
               this.setState({ [dataType]: e.target.value })
           }
       }
   
       login1 = (dataType, e) => {
           this.setState({ [dataType]: e.target.value })
       }
   
       render() {
           return (
               <section>
                   用户名: <input type="text" onChange={this.login('username')} />
                   <br />
                   密码: <input type="text" onChange={(e) => this.login1('psd', e)} />
                   <br />
                   <input type="button" value="登陆" onClick={this.showData} />
               </section>
           )
       }
   }
   
   ReactDOM.render(<InputUse />, document.querySelector('#app'))
   ```

8. 生命周期: <font color="red">(旧版)</font>
   
   <img src="./笔记资源/(旧)React生命周期.png" title="(旧)React生命周期" alt="(旧)React生命周期.png" data-align="inline">
   
   ```jsx
   // 组件卸载(注意不是生命周期钩子)
   ReactDOM.unmountComponentAtNode(document.querySelector('#app'));
   ```

9. 生命周期: <font color="red">(新版)</font>
   ![(新)React生命周期.png](./笔记资源/(新)React生命周期.png "(新)React生命周期")

10. 脚手架环境
    
    1. 全局安装: ` npm install -g create-react-app`
    
    2. 在要使用的文件夹内创建: ` crate-react-app <project-name>`
    
    3. 进入项目文件夹: ` cd <project-name>`
    
    4. 启动项目: ` npm start`
    
    5. 文件
       
       1. index.js: 入口文件
       
       2. App.js: 组件总入口文件爱你
       
       3. src/component/\<ComponentName\>: 组件放置规范
       
       4. 组件文件夹内文件规范
          
          1. 组件文件: .jsx
          
          2. 样式文件: .css
          
          3. 普通js文件: .js

11. src的基本文件配置
    
    + index.js
      
      ```jsx
      import React from "react";
      import { createRoot } from 'react-dom/client';  // 特别注意, 现在不是直接导入 ReactDOM 库
      import App from "./App";
      
      const root = createRoot(document.querySelector('#root'));
      root.render(
          <React.StrictMode>
              <App></App>
          </React.StrictMode>
      );
      ```
    
    + App.js
      
      ```jsx
      import React, { Component } from "react";
      import Hello from "./components/Hello";
      
      export default class App extends Component {
          render() {
              return (
                  <div className="main-wrap">
                      <Hello></Hello>
                  </div>
              )
          }
      }
      ```

12. 插件快捷键
    
    1. rcc : 类式组件
    
    2. rfc: 函数式组件
    
    3. [提示快捷键](./笔记资源/Snippets.md)

13. 小结<font color="red">(可能以后这个总结是含有废弃项或不推荐项的)</font>
    
    1. 数据维护: 统一在App或某同胞组件的父组件上, 利用 props 来传, 由App统一管理
       
       ```jsx
       // App.js
       state = {
            taskList: [
                {
                    id: 1,
                    name: '吃饭',
                    isDone: true
                },
                {
                    id: 2,
                    name: '睡觉',
                    isDone: false
                },
            ],
        }
       
       // Child.jsx
       // 接收
       ```
    
    2. 父子组件通信: 在父组件上, 定义一个准备接收值的函数, 通过props传递给子组件; 再由子组件使用该函数, 
       
       ```jsx
       // App.js
       getKeywords = (value) => {
           // value 用于 获取子组件传递的值
           // 可以理解为: <Com @getKeywords="getKeywords" />
       }
       
       <CenterWrap taskList={this.state.taskList} />
       
       // Child.js
       this.props.getKeywords(value); // 可以理解为 this.$emit('getKeywords', value);
       ```
    
    3. 随机唯一id/标识库: ` npm i nanoid`
    
    4. 用state维护组件状态, 尽量少用ref
       
       ```jsx
       // 可以直接当作 v-show 的原型
       
       // html
       <section
           className='sg-task-wrap'
           style={isShow ? { backgroundColor: '#ccc' } : { backgroundColor: 'initial' }}
           onMouseOver={this.handleDeleteBtn(true)}
           onMouseLeave={this.handleDeleteBtn(false)}
       />
       
       // 函数
       handleDeleteBtn = (flag) => {
           const { isShow } = this.state;
           if (isShow && flag == isShow) return;
       
           return () => {
               this.setState({ isShow: flag })
           }
       }
       ```
    
    5. props类型限制: 
       
       1. 安装: ` npm i prop-types`
       
       2. 使用(该包上的方法很玄学, 后面再说)
          
          ```jsx
          // 
          ```
    
    6. 删除对象指定的键: delete obj.a()<font color="orange">(一般用filter来返回新数组)</font>
    
    7. 数组统计或相加
       
       ```js
       // 1. 求和
       const arr = [1, 2, 4];
       arr.reduce((pre, cur) => { return pre + cur })
       
       // 2. 计数
       const arr = [1, 2, 4];
       arr.reduce((pre, cur) => { return pre + cur }, 0/[index])
       ```
    
    8. 

14. 脚手架代理配置(即解决跨域)
    
    1. 方式一<font color="orange">(只能配置一个, 所以有局限性)</font>
       
       ```jsx
        // 1. 在 package.js 配置, 加上一句
        "proxy": "http://服务器地址:代理端口"
       ```
    
    2. 方式二<font color="orange">(能配置多个, 但一般只需要配置一个, 并且这个比较麻烦)</font>
       
       ```js
       // 1. src/ 下, 新建名为 setup.js 的文件
       // setup.js
       const proxy = require('http-proxy-middleware');
       
       module.exports = function(app) {
           app.use(
               proxy('/api', {
                   target: "http://服务器地址:代理端口",
                   changeOrigin: true,
                   pathRewrite: {'^/api': ''}
               })
           )
       }
       ```

15. 兄弟组件间通信: 消息订阅发布组件
    
    1. 安装
       
       ```shell
       npm i pubsub-js
       ```
    
    2. 使用
       
       ```jsx
       // 1. 引入
       import PubSub from 'pubsub-js'
       
       // 2. Hello.jsx (消息订阅组件)
       componentDidMount() {
           PubSub.subscribe('Hello', (msg, data) => {
               console.log(msg, data);
           })
       }
       
       // 3. Welcome.jsx (消息发布组件)
       publishMsg = () => {
           PubSub.publish('Hello', { ...this.state })
       }
       ```

16. 路由<font color="orange">这里学的是Web, 并且是Router5(分为Web, Native 和 any)</font>(看后面的6)
    
    1. 安装: ` npm i react-router-dom@5`
    
    2. 路由跳转(基本使用)<font color="orange">(基本使用, 不是最佳实践)</font>
       
       ```jsx
       // 路由注册核心: <Route path="path" component={comA}>
       
       // 路由链接核心: <Link to="path">content</Link> 或 
       //              <NavLink to="path">content</NavLink >
       
       // 1. 引入
       import { Link, <BrowserRouter || HashRouter> } from 'react-router-dom'
       
       // 2. 编写路由链接
       <BrowserRouter >
           <Link to="/about">About</Link>|
           <Link to="/home">Home</Link>
       </BrowserRouter>
       
       // 3. 注册路由
       <BrowserRouter>
           <Route path="/about" component={About}></Route>
           <Route path="/home" component={Home}></Route>
       </BrowserRouter>
       ```
    
    3. 路由跳转最佳实践(因为只能利用同一个Router)
       
       ```jsx
       // 1. 在 index.js 内全局注册
       root.render(
           <React.StrictMode>
               <BrowserRouter>
                   <App />
               </BrowserRouter>
           </React.StrictMode>
       )
       
       // 2. 然后就可以拆分 导航 和 页面
       ```
    
    4. 路由跳转, 且导航变色
       
       ```jsx
       // 1. 引入 NavLink 组件
       import { NavLink } from 'react-router-dom'
       
       // 2. 使用, activeClassName如果不写这个属性, 默认追加类名 .active
       <NavLink to="/about" [activeClassName="active || ..."]>About</NavLink>
       
       // 3. 添加 .active 样式
       .sider-wrap a {
         padding: 1rem;
         color: black;
         text-decoration: none;
         border: 1px solid #ccc;
         border-radius: 8px;
       }
       .sider-wrap a.active {
         background-color: aqua;
       }
       ```
    
    5. 封装NavLink(即在HTML中, 便利与简洁如Vue)
       
       ```jsx
       import React, { Component } from 'react'
       
       import './index.css'
       
       import { NavLink } from 'react-router-dom/cjs/react-router-dom.min'
       
       export default class MyNavLink extends Component {
           render() {
               return (
                   第一种方式, 因为标签体内容也是特殊的
                   <NavLink
                       {...this.props}
                       className="nav-link"
                       activeClassName="active"
                   />
       
                   第二种, 没必要这样写
                    <NavLink
                        {...this.props}
                        className="nav-link"
                        activeClassName="active"
                    >
                        {this.props.children}
                    </NavLink>
               )
           }
       }
       ```
    
    6. 路由只匹配第一个出现的路由配置
       
       ```jsx
       // 核心: <Switch></Switch>
       export default class RouterView extends Component {
           render() {
               return (
                   <Switch>
                       <Route path="/home" component={Home} />
                       <Route path="/about" component={About} />
                   </Switch>
               )
           }
       }
       ```
    
    7. 路由模糊匹配与精准匹配
       
       ```jsx
       // 模糊匹配: 默认, 只要第一个 /xxx 能匹配上, 就不管后面还有什么 /aaa 都能返回 /xxx 的路由
       
       // 精确匹配: 核心: exact={true}
       // 如果没有出现问题, 尽量不要开严格匹配
       export default class RouterView extends Component {
           render() {
               return (
                   <Switch>
                       <Route exact={true} path="/home" component={Home} />
                       <Route exact path="/about" component={About} />
                   </Switch>
               )
           }
       }
       ```
    
    8. 网页开启时的默认路由/地址设置, 及404匹配
       
       ```jsx
       // 默认地址核心: <Redirect to="path" />
       
       // 404匹配核心: <Route path="*" component={comA}></Route>
       
       import { Route, Switch, Redirect } from 'react-router-dom/cjs/react-router-dom.min'
       
       export default class RouterView extends Component {
           render() {
               return (
                   <Switch>
                       <Route path="*" component={NotFound}></Route>
                       <Redirect to="/home" />
                   </Switch>
               )
           }
       ```
    
    9. 嵌套路由(多级路由)<font color="orange">(可能只是暂时这样写)</font>
       
       ```jsx
       // 1. 哪里定义链接, 哪里注册路由
       ```
    
    10. 传递params(用的比较多)
        
        ```jsx
        // 路由设置
        <Route path="/home/message/details/:id/:title" component={Details} />
        
        // 1. 静态传参
        <MyNavLink to={`/home/message/details/004/m4`} >test</MyNavLink>
        
        // 2. 动态传参
        <MyNavLink key={item.id} to={`/home/message/details/${item.id}/${item.title}`} >
            {item.title}
        </MyNavLink>
        
        // 3. 子组件接收
        this.props.match.params
        ```
    
    11. 传递search参数
        
        ```jsx
        // 路由设置
        <Route path="/home/message/details" component={Details} />
        
        // 传参
        <MyNavLink to={`/home/message/details?id=${'001'}&title=${'test'}`} >test</MyNavLink>
        
        // 子组件接收
        this.props.location.search
        
        // 注意, search参数需要自己处理
        import qs from 'querystring'
        
        let search = ">id=001&title=test";
        let obj = {
            "id": "001",
            "title": "test"
        }
        
        qs.parse(search.slice(1))  // search 转 obj
        qs.stringify(obj) // obj 转 search
        ```
    
    12. 传递state参数
        
        - 不是同一个state
        
        - 强刷会丢, 普刷不会, 优点就是地址栏不显示
        
        - hash的state的普刷也会直接丢
        
        ```jsx
        // 路由设置
        <Route path="/home/message/details" component={Details} />
        
        // 传参(注意pathname是小写)
        <MyNavLink
            to={{
                pathname: '/home/message/details',
                state: { id: '005', title: "m5" }
            }}
        >
            state
        </MyNavLink>
        
        // 接收
        this.props.location.state
        ```
    
    13. push和replace模式
        
        ```jsx
        // 1. push模式(默认)
        <Route path="/home/message/details" component={Details} />
        this.props.history.push('path');
        
        // 2. replace模式
        <Route replace={true} path="/home/message/details" component={Details} />
        this.props.history.replace('path');
        ```
    
    14. 一般组件使用路由api
        
        ```jsx
        // 1. 引入内置函数
        import { withRouter } from 'react-router-dom/cjs/react-router-dom.min'
        
        // 2. 核心: 将处理过后的一般组件暴露, 然后这个组件就可以读到路由的api了
        export default withRouter(Header);
        ```

17. 组件的标签体内容传递:  利用 props 的 children

18. redux<font color="orange">(能不用就不用)</font>(状态管理, 和本家没有关系, 就像js和java的区别)
    
    1. (暂存)
    
    2. 1
    
    3. 1
    
    4. 1
    
    5. 

----

## 二. 扩展

1. setState()的两种写法
   
   - 如果依赖原状态, 则一般使用函数式
   
   - 不依赖, 一般使用对象式
     
     ```jsx
     // 第一种: 对象式 setState()
     // 核心: this.setState({k: v}, callback)
     addCount = () => {
        let { count } = this.state;
     
        this.setState({ count: ++count }, () => {
            console.log(this.state.count);
        });
     }
     
     // 第二种: 函数式 setState()
     // 核心:  
      this.setState((state, props) => {
          return { count: state.count + 1 }
      }, [callback])
     ```

2. 路由组件懒加载
   
   ```jsx
    import {lazy, Suspense} from 'react';
   
    const LazyCom = lazy(() => {
        return import('./components/02_lazyload')
    })
   
    <Suspense fallback={<h1>Loading.....</h1>}>
        <Routes>
            <Route path='/01_setState' Component={setStateCom} />
            <Route path='/02_lazyload' Component={LazyCom} />
        </Routes>
    </Suspense>
   ```

3. Hooks
   
   1. useState()
      
      ```jsx
      // 核心: const [count, setCount] = useState(0);
      
      import React, { useState } from 'react'
      
      export default function Hooks() {
          const [count, setCount] = useState(0);
      
          function add() {
              setCount(count + 1);
          }
      
          return (
              <section>
                  <span>值:{count} </span>
                  <br />
                  <button onClick={add}>+</button>
              </section>
          )
      }
      ```
   
   2. useEffect(): 监测离开页面、刷新页面
      
      ```jsx
      // 核心: useEffect()
      useEffect(() => {
          // 该函数体, 可等价为didMount
          console.log('mount');
      
          // 更新时, 该函数体, 可等价为update
          console.log('update');
      
          return () => {
              // 该函数体, 可等价为 willUnmount
              console.log('unmount');
          }
      
      // 数组内, 即监视那些变量
      // 若不传第二个参数(即[count]位置), 则为监视所有
      // 若数组内没有任何变量, 则全部不监视
      }, [count]);
      ```
   
   3. useRef()
      
      ```jsx
      const inputRef = useRef();
      
      function showText() {
          console.log(inputRef.current.value);
      }
      
      return (
          <section>
              <input ref={inputRef} type="text" />
              <button onClick={showText}>提示数据</button>
          </section>
      )
      ```

4. Context
   
   + 核心:` const MyContext = createContext();`
   
   + 要在所有子组件能访问到的地方声明该变量
   
   + 包裹子组件: ` <React.Provider value={}> <Child/> </React.Provider>`
   
   + 在子组件中声明: ` static contextType = MyContenxt`
   
   + 然后在子组件中, 就可以直接使用 this.context 获取传的值
     
     ```jsx
     // 一定要在所有子组件能放问到的地方声明变量
     const MyContext = createContext();
     const { Provider } = MyContext;
     
     class A extends Component {
        state = {
            name: 'tom',
            age: 18,
        }
     
        render() {
            const { name, age } = this.state;
     
            return (
                <div className='a'>
                    A组件,  <h1>name:{name}</h1>
                    <br />
                    <Provider value={{ name, age }}>
                        <B />
                    </Provider>
                </div>
            )
        }
     }
     class B extends Component {
        render() {
            return (
                <div className='b'>
                    B组件
                    <br />
                    <C />
                </div>
            )
        }
     }
     
     class C extends Component {
        static contextType = MyContext
        render() {
            const { name, age } = this.context;
            return (
                <div className='c'>C组件<h1>name: {name}</h1></div>
            )
        }
     }
     ```
   - 如果是函数式式组件
     
     - 父组件(和类式组件一致): \<Provider></Provider>
     
     - 子组件: \<Consumer>{ value => {  } }</Consumer>
     
     ```jsx
     const MyContext = createContext();
     const { Provider, Consumer } = MyContext;
     
     class A extends Component {
        state = {
            name: 'tom',
            age: 18,
        }
     
        render() {
     
            const { name, age } = this.state;
     
            return (
                <div className='a'>
                    A组件,  <h1>name:{name}</h1>
                    <br />
                    <Provider value={{ name, age }}>
                        <B />
                    </Provider>
                </div>
            )
        }
     }
     
     function C() {
        return (
            <div className='c'>C组件
                <h1>
                    <Consumer>
                        {
                            value => `name: ${value.name}, age: ${value.age}`
                        }
                    </Consumer>
                </h1>
            </div>
        )
     }
     ```

5. renderProps()
   
   + 什么情况使用? 
     组件在祖组件, 直接嵌套<子和孙组件>
     
     ```jsx
     <A>
         <B></B>
     </A>
     ```
   
   + 祖组件
     
     ```jsx
     <A render={name => <B name={name} />} />
     ```
   
   + 子组件
     
     ```jsx
     {this.props.render(name)}
     ```
   
   + 孙组件
     
     ```jsx
     {this.props.name}
     ```
   
   + 完整展示
     
     ```jsx
     import React, { Component } from 'react'
     
     import './04_context.css'
     
     class A extends Component {
      state = {
          name: 'tom',
          age: 18,
      }
     
      render() {
          const { name } = this.state;
     
          return (
              <div className='a'>
                  A组件
                  {this.props.render(name)}
              </div>
          )
      }
     }
     class B extends Component {
      render() {
          console.log(this.props);
          return (
              <div className='b'>
                  B组件, name: {this.props.name}
              </div>
          )
      }
     }
     
     export default class RenderProps extends Component {
      render() {
          return (
              <div>
                  <A render={name => <B name={name} />} />
              </div>
          )
      }
     }
     ```

---

## 三. Route6

1. router_6

2. 路由注册
   
   - <mark>Routes</mark>代替了<mark>Switch</mark>
   - Route的<mark>element</mark>代替了<mark>component</mark>
   
   ```jsx
   import React, { Component } from 'react'
   import { NavLink, Route, Routes } from 'react-router-dom'
   
   export default class App extends Component {
       render() {
           return (
               <div>
                   <ol>
                       <li><NavLink to="/">A</NavLink></li>
                       <li><NavLink to="/b">B</NavLink></li>
                   </ol>
   
                   <Routes>
                       <Route path='/a' element={<A />}></Route>
                       <Route path='/b' element={<B />}></Route>
                   </Routes>
               </div>
           )
       }
   }
   
   class A extends Component {
       render() {
           return (
               <div>ComA</div>
           )
       }
   }
   
   class B extends Component {
       render() {
           return (
               <div>ComB</div>
           )
       }
   }
   ```

3. 路由重定向
   
   - <mark>\<Navigate to="/path"></mark>代替了<mark>\<Redirect></mark>
   - Navigate是一个组件, 放在Route组件中
   
   ```jsx
   import { Navigate, NavLink, Route, Routes } from 'react-router-dom'
   
   <Route path='/' element={<Navigate to='/b' />} />
   ```

4. 路由链接高亮显示
   
   - 废弃了<mark>activeClassName</mark>
   
   - NavLink的className会给出一组status值, 利用其中的isActive来进行判断
   
   ```jsx
   export default function App() {
       function computeClassName(status) {
           return status.isActive ? 'router router-actived' : 'router'
       }
   
       return (
           <div className='app'>
               <ol>
                   <li><NavLink className={computeClassName} to={"/a"}>A</NavLink></li>
                   <li><NavLink className={computeClassName} to={"/b"}>B</NavLink></li>
               </ol>
   
               <Routes>
                   <Route path='/a' element={<A />} />
                   <Route path='/b' element={<B />} />
                   <Route path='/' element={<Navigate to='/b' />} />
               </Routes>
           </div>
       )
   }
   ```

5. useRoute(): 路由表
   
   - 可以直接单独配置路由表了(类似Vue)
     
     ```jsx
     export default [
         {
             path: '/a',
             element: <A />
             children: [
                 {
                     path: 'c',
                     element: <C />,
                 },
             ]
         },
         {
             path: '/b',
             element: <B />
         },
         {
             path: '/',
             element: <Navigate to="/a" />
         },
     ]
     ```
   
   - 组件内使用
     
     ```jsx
     import {  useRoutes } from 'react-router-dom'
     import myRoutes from './routes'
     
     const routes = useRoutes(myRoutes);
     
     // 无需再用<Routes>来包裹了
     {elements}
     ```
   
   - 嵌套路由的指定呈现位置(类似Vue的<mark>\<router-view></mark>)
     
     - 注意NavLink路径
       
       - 原路径: /a/c, 但已经在路由表中配置了, 所以注意这个子路由就只能写 c
       
       - 所以: to="c"
     
     ```jsx
     import { Outlet } from 'react-router-dom'
     
     // 原路径: /a/c, 但已经在路由表中配置了, 所以注意这个子路由就只能写 C
     <NavLink className={computeClassName} to="c">C</NavLink>
     
     // 直接渲染即可, 使用方式和Vue一样
     <Outlet/>
     ```

6. 路由传参
   
   1. params
      
      - 接收参数: useParams()
      
      ```jsx
      // 路由表配置
      {
          path: 'detail/:id/:msg',
          element: <Details />
      }
      
      // 路由链接
      <NavLink to={`detail/${item.id}/${item.msg}`}>
          <span>{item.id}: </span>
      </NavLink>
      
      // 子组件接收参数
      function Details() {
          const { id, msg } = useParams();
      
          return (
              <div>
                  <span>{id}:</span>
                  <span>{msg}</span>
              </div>
          )
      }
      ```
   
   2. search
      
      - 接收参数: const [search, setSearch] = useSearchParams();
      
      - 注意该函数不是直接能返回 query 值或参数
      
      - 需要使用 search.get('key') 来返回值
      
      ```jsx
      // 路由表配置
      {
          path: 'detail',
          element: <Details />
      }
      
      // 路由链接
      <NavLink to={`detail?id=${item.id}&msg=${item.msg}`}>
          <span>{item.id}: </span>
      </NavLink>
      
      // 子组件接收参数
      function Details() {
          const [search, setSearch] = useSearchParams();
          const id = search.get('id');
          const msg = search.get('msg');
      
          return (
              <div>
                  <span>{id}:</span>
                  <span>{msg}</span>
              </div>
      }
      ```
   
   3. state
      
      - 接收参数: useLocation().state
      
      - NavLink写法有变: 即把pathname和state分开了
      
      - <mark>\<NavLink to="detail" state={{ id: item.id, msg: item.msg }}/></mark>
      
      ```jsx
      // 路由表配置
      {
          path: 'detail',
          element: <Details />
      }
      
      // 路由链接
      <NavLink to="detail" state={{ id: item.id, msg: item.msg }}>
          <span>{item.id}: </span>
      </NavLink>
      
      // 子组件接收参数
      function Details() {
          const [search, setSearch] = useSearchParams();
          const id = search.get('id');
          const msg = search.get('msg');
      
          return (
              <div>
                  <span>{id}:</span>
                  <span>{msg}</span>
              </div>
      ```

7. 编程式路由(路由跳转)
   
   - 指定跳转
     
     ```jsx
     const navigate = useNavigate();
     
     function to() {
         navigate('/a/detail', {
             replace: false,
             state:
             {
                 id: '002',
                 msg: '第一条消息',
             },
         })
     }
     ```
   
   - 前进后退
     
     ```jsx
     function forward() {
         navigate(1)
     }
     
     function back() {
         navigate(-1)
     }
     ```

8. 
