#### 1. duck-type :  has-A 而不是 is-A

#### 2.面向对象

    (1) 变量 -> 函数式

```js
 let v = function() {};
 v.prototype.property = function() {
     // function body
 }
```

     (2) 函数的 数组存储 与 封装调用

```js
 const arr = [];
 // func
 const func = function(args) {
     if (// check property) {
         arr.push(args);
     }
 }
 // instance 
 func( new v );
```

#### 3.多态继承

        (1) 多态 -> 将 "做什么" 与 "谁去做" 分离 

        (2) 作用 -> 将 面向过程化 的分支语句, 转换为 对象形式的多态性, 消除分支语句  (3) 由 (2) 得:

```js
 面向方法: if( map === 'google' ) {} else if (map === 'baidu')
 面向对象: if( map.value/type/fun instances of Function/... );
```



随记: 
    (1)    propertype 属性指向对象, 这个对象即调用 构造函数 而创建的 实例 的原型( eg: new Person() 这个就是对象原型)
    (2) instances of xxx 就是 利用原型链原理, 找到这个东西是否有存在这个实例及实例中的属性



#### 4.封装

    (1) 封装(?闭包) (这个指 数据封装)

```js
 let x = (function() {
     let __name = 'Lumina';  // 私有成员
     return { // 公开成员
         getName() { return __name }
     }
 })();
 // 思考后的写法
 class Constuctor { constructor() {}, fun() }
 // 其中 super() 继承, 是需要在 constructor() {} 中使用, 形参需要一并写入
 // 其中, fun 可以这样写:
 Constructor.prototype.fun = function() {}
```

 (2) 原型对象克隆

```js
 Object.create( OriginObject );
 原型对象的克隆, 若这个函数(对象)是作为构造函数使用的, 对象实例化后为进行数值构造那么仅仅只是克隆相应的结构(形参与实例化后传参无用); 若实例化后进行构造, 那么会将其函数构造后的数据一起克隆.
```

 (3) 原型对象继承

```js
 // 旧版(?或者说 extends 原理, ?new 原理(书上=new) )
 function Person(name) {
     this.name = name;
 }

 Person.prototype.getName = function() {
     return this.name;
 }

 const objectFactor = function() {
     let obj = new {};
     let Constructor = [].shift.call( arguments );
     obj.__proto__ = Constructor.prototype;

     let ret = Constructor.apply( obj, arguments );

     return typeof === ret ? ret : obj;
 }
 let a = objectFactor(Person, 'sven');
  // -------
  .apply(obj, arguments): 调整 对象 + 参数 的 指向与类型(便于内部构造), 并应用于构造
  .call( arguments ): 将所有 原型的参数 导入构造函数, 将 this 指向调用此方法的函数
```

##### 5.this & call & apply

 (1) call & apply

```js
 .apply() : 
   按照 传递的参数执行, 会忽略 Null 值;
   会对数组中的元素进行 按照次序,等量 对应拆分(形参有几个就只拆成几个); 
   如果形参为 对象 形式, 那么 形参都为 undefined.(?不支持传递对象)

 .call()  : 
   apply() 上的一颗语法糖, 严格按照形参传递执行. 
   不对应的 args 被定义为 undefined
```

 (2) 这两个 会改变 this 指向

```js
 eg: 
 getName();  // window 的  name
 getName.call(obj);  // obj 内的 name
```

 (3) 这两个的主要作用是用于改变 this 指向
 (4) 一些方法可以进行缩写, 不过注意指向的 this 对象:

```js
 eg: 
 document.querySelector = (function(func) {
     return function() {
         return func.apply(document, arguments);
     }
 })()
```

 (5) .bind()  // 改变 func 作用域

```js
 Function.prototype.bind = function() {
     let self = this, 
         context = [].shift.call(arguments), 
         args = [].splice.call(arguments);

     return function() {
         return self.apply(context, [].concat.call(args, [].splice.call(arguments)))
     }
 }

 const obj = { name: "Lumina"}
 const func = function() {  }.bind(obj)   // log: this.name => Lumina
```

#### 6. 闭包

(1) (function() {})()

```js
 注: 后面的那个 括号 是指传入的实际参数(?官方称运行函数)
 (只有 var 会出现这种问题) => fori 中绑定的每个按钮内 的 i 值 都是最后的 i 值
 fori {
    (function(i) {
        btns[i].addevent('click', func() {
            log(i)  ==> 此时的 i 值就是 btn 的索引
        })
    })(i) 
 }
```

 (2) 变量也可利用闭包去拿值

```js
 const arr = (function() { 
     fori { temp.push(i) return temp; } 
 })();
```

 (3) const func = function() { let a = 0; return a; } 

##### 7.高阶函数 => AOP (面向切面编程)

    (1) 柯里化 ( Currying(fn) )

```js
 简单理解为 => 事件的统计, 分次进行统计, 等需要进行输出时进行一次无参调用
 柯里化: 当 传入参数长度非 0 时, 将 fn 进行柯里化
 柯里化原理: 鸭子类型
 柯里化代码逻辑: 
     args = [];

     if: arguments.length === 0
       return fn.apply(this, args);  // 将统计的参数, 转换为 fn 的实际参数
     else: 
       [].push.apply(args, arguments);  // fn 中含实际参数时, 将这些参数 push 进 args
       return arguments.callee;  // 返回这些参数运行函数(想表达的是:  返回fn(param), 而不是返回 fn )

!注!: 进行柯里化 的所谓的 fn 是一个闭包中 return的func; 需要注意的是,  因为 (?本身意义)仅是作为统计 所以逻辑本身与统计相关, 只是 统计 或 暂存 状态
```

 (2) 节流

```js
 1) 传参 => 标准是 timeout | timeinterval  // (fn, interval)
 2) 内置参数
    _self = fn;  // 指向 fn 本身
    timer;  // 定时器
    firstTime = true;  // 是否第一次触发, 避免第一次触发时需等待 interval
 3) 函数构造(函数实体)
    rt func() {
        _me = this;
        args = arguments;

        if(firstTime) {  // 首次延迟避免
           _self.apply(_me, args);
           return firstTime = false;
        }

        if(timer) {  // 是否仍处定时状态, 所以记得置空
            return false;
        }

        timer = setTimeout(() => {
            clearTimeout(timer);
            timer = null;
            _self.apply(_me, args);
        }, interval || 500);
    }
```

##### 8.单例模式

(1) 单例: 保证一个类仅有一个访问实例, 并提供一个访问它的全局访问点.  

(2) 惰性单例: 在元素需要时, 才进行元素创建

```js
const btn = document.querySelector('#login');

const createLoginLayer = (function () {
    let div;
    return function () {
        if (!div) {
            div = document.createElement('div')
            div.innerHTML = 'Login';
            div.style.display = 'none';
            document.body.appendChild(div);

        };

        return div;
    }
})()

btn.addEventListener('click', function () {
    const loginLayer = createLoginLayer();

    loginLayer.style.display = 'block';
})
```

##### 9. 策略模式

1. 定义:  定义一系列算法, 把他们一个个封装起来, 并使他们可以相互替换

2. 例子: 
   
   ```js
   const strategies = {
       S(salary) {
           return salary * 4;
       },
       A(salary) {
           return salary * 3;
       },
       B(salary) {
           return salary * 2;
       }
   }
   
   const calculate = function (level, salary) {
       return strategies[level](salary);
   }
   ```

3. 主要使用场景: 多重条件选择语句
   比如: 登陆, 数据验证, 

##### 10. 代理模式

1. 定义: 为一个对象提供一个代用品或占位符, 以便控制对他的访问

2. 可以简单理解为: 现实中的中介
   
   1. 客户(主体): 提供房子的要求...
   2. 中介(客体): 返回对应需求的结果,
   3. 因为中介需要中介费, 所以实际上多出的客体逻辑, 会比不进行代理逻辑的代码量高.

3. 由 1 得, 那么就能分为 主体 与 客体
   
   1. 主体: 用于 实体逻辑. 主体是主要执行者, 对需要代理的东西提供外部接口
   2. 客体: 用于 代理逻辑. 对主体提供的接口进行逻辑实现

4. 在代理模式中, 因为主体也有相同逻辑接口, 所以当代理不需要时, 主体仍可继续使用

5. 虚拟代理
   
   1. 对主体功能进行代理, 很像 xxx.prototype.xxx = xxxx;
   
   2. 接口与代理保持一致
   
   3. 代理基本为闭包
   
   4. 图片预加载 + 合并 http 请求
      
      ```js
      
      const MyImg = (function () {
              const imgNode = document.createElement('img');
              document.body.appendChild(imgNode);
      
              return {
                  setSrc(src) {
                      imgNode.src = src;
                  }
              }
          })();
      
          const proxyImage = (function () {
              const img = new Image;
      
              img.onload = function () {
                  MyImg.setSrc(this.src);
              }
      
              return {
                  setSrc(src) {
                      MyImg.setSrc('../temp/imgs/loading.gif');
                      img.src = src;
                  }
              }
          })();
      
            proxyImage.setSrc('../temp/imgs/LoginBg.jpg');
      
            // #region 虚拟代理 ---- 合并HTTP请求
            const syncFile = function (id) {
                console.log('开始同步文件, id 为: ' + id);
            }
      
            const proxySync = (function () {
                let caches = [], timer;
      
                return function (id) {
                    caches.push(id);
      
                    if (timer) return;
      
                    timer = setTimeout(() => {
                        syncFile(caches.join(','));
                        clearTimeout(timer);
                        timer = null;
                        caches.length = 0;
                    }, 2000)
                }
            })();
      
            const checkbox = document.querySelectorAll('input');
            for (let i = 0; i < checkbox.length; i++) {
                checkbox[i].addEventListener('click', function () {
                    if (this.checked === true) proxySync(this.id)
                })
            }
            // #endregion
      ```

6. 缓存代理
   
   1. 对之前进行过的操作进行缓存, 在下次再次进行同样操作时, 直接返回之前的缓存
   
   2. 计算
      
      ```js
      
          const mult = function () {
              console.log('开始计算乘积');
              let a = 1;
      
              for (let i = 0; i < arguments.length; i++) {
                  a *= arguments[i];
              }
      
              return a;
          }
      
          const proxyMult = (function () {
              let caches = {};
      
              return function () {
                  let args = Array.prototype.join.call(arguments, ',');
                  if (args in caches) return caches[args];
                  return caches[args] = mult.apply(this, arguments);
              }
          })();
      ```

##### 11.迭代器模式

1. (不咋算一种设计模式) 对 arr, obj 的迭代操作, 类似 foreach, filter....

2. 内部迭代器: 固定的迭代, 针对某类情况而设计
   
   ```js
   const each = function (ary, callback) {
       for (let i = 0; i < ary.length; i++) {
           callback.call(ary[i], i, ary[i]);
       }
   }
   
   each([1, 2, 3], function (i, n) {
       console.log(i + '=>' + n);
   })
   ```

3. 外部迭代器: 业务性的迭代, 偏实用性, 以业务需求为主要导向
   
   ```js
    const Interator = function (obj) {
        let current = 0;
   
        function next() {
            current += 1;
        }
   
        function isDone() {
            return current >= obj.length;
        }
   
        function getCurrItem() {
            return obj[current];
        }
   
        return {
            next,
            isDone,
            getCurrItem
        }
    }
   
    const compare = function (interator1, interator2) {
        while (!interator1.isDone() && !interator2.isDone()) {
            if (interator1.getCurrItem() !== interator2.getCurrItem()) {
                throw new Error('interator1 和 interator2 不相等')
            }
   
            interator1.next();
            interator2.next();
        }
   
        console.log('interator1 和 interator2 相等');
    }
   
    const interator1 = Interator([1, 2, 3]);
    const interator2 = Interator([3, 2, 3]);
   
    compare(interator1, interator2);
   ```

##### 12. 观察者模式( 发布 - 订阅模式 )

1. 观察者模式可分为两种( 一般以第二种为准 )
   
   1. 订阅者 + 发布者
   
   2.  订阅者 => 中介 <= 发布者

2. 订阅者 => 中介 <= 发布者 : 楼房销售案例

```js
const salesOffices = {
    clientList: {},

    listen(key, fn) {
        if (!this.clientList[key]) this.clientList[key] = [];

        this.clientList[key].push(fn);
    },

    remove(key, fn) {
        const fns = this.clientList[key];

        if (!fns) return false;

        if (!fn) {
            // fns && (fns.length = 0);
            (fns.length = 0);
        } else {
            for (let i = fns.length - 1; i >= 0; i--) {
                let _fn = fns[i];

                if (_fn === fn) fns.splice(i, 1);
            }
        }
    },



    trigger() {
        const key = Array.prototype.shift.call(arguments),
            fns = this.clientList[key];

        if (!fns || fns.length === 0) {
            return false;
        }

        for (let i = 0, fn; fn = fns[i++];) {
            fn.apply(this, arguments);
        }
    }
};

salesOffices.listen('squareMeter88', fn1 = function (price) {
    console.log(arguments.callee.name, '价格' + price);
});

salesOffices.listen('squareMeter88', fn2 = function (price) {
    console.log(arguments.callee.name, '价格' + price,);
});

salesOffices.remove('squareMeter88', fn1)
salesOffices.trigger('squareMeter88', 888888);

const installEvent = function (obj) {
    for (const k in salesOffices) {
        obj[k] = salesOffices[k];
    }
};

const newSalesOffice = {};
installEvent(newSalesOffice);
```

4. 订阅者 => 中介 <= 发布者 : 模块间通信案例

```js
const count = {
    countList: [],

    listen(key, fn) {
        if (!this.countList[key]) this.countList[key] = [];

        this.countList[key].push(fn);
    },

    trigger() {
        let key = [].shift.call(arguments), fns = this.countList[key];

        if (!fns || fns.length === 0) return false;

        for (let i = 0, fn; fn = fns[i++];) {
            fn.apply(this, arguments);
        }
    }
}

const a = (function () {
    let x = 0;

    document.querySelector('input').addEventListener('click', function () {
        count.trigger('add', ++x)
    });
})()

const b = (function () {
    count.listen('add', function (data) {
        b.setAddRes(data);
    });


    return {
        setAddRes(data) {
            document.querySelector('.count').innerHTML = data;
        }
    }
})();
```

##### 13.命令模式

1. 将命令一个个分解开, 有点类似于 策略模式, 稍有不同的仅在 命令执行 || 命令制作 时是否含有 接收者

2. 命令模式分为两种
   
   1. 智能命令(不含接收者)
      
      ```js
      const ballStatus = {
          elem: document.querySelector('.ball'),
      
          recordStack: [],
      
          makeCommand(step) {
              this.elem.style.left = step + 'px';
          },
      
          recall() {
              if (this.recordStack.length === 0) return false;
      
              for (let i = 0, step = 0, fn; step += 500, fn = this.recordStack.shift();) {
                  setTimeout(() => {
                      fn();
                  }, step);
              }
      
          },
      
          exec() {
              let firstTime = false,
                  _this = this;
      
              return function () {
                  let left = _this.elem.style.left || '0';
                  let step = Number(left.split('px')[0]);
      
                  if (_this.recordStack.length === 0) {
                      _this.makeCommand(step + 10);
                      _this.recordStack.push(function () {
                          _this.makeCommand(left);
                      });
                      firstTime = true;
                  } else {
                      _this.makeCommand(step + 10);
                      _this.recordStack.push(function () {
                          _this.makeCommand(step + 10);
                      });
                  }
              }
          },
      
          undo() {
              if (this.recordStack.length !== 0) {
                  let fn = this.recordStack.pop();
                  fn();
              }
          }
      }fn();
                      }
                  }
              }
      ```
      
      

2. 傻瓜命令(宏命令)
   
   ```js
   const command = {
       closeDoor() {
           console.log('关门');
       },
       openPC() {
           console.log('开电脑');
       },
       openQQ() {
           console.log('登QQ');
       }
   }
   
   class MacroCommand {
       constructor() {
           this.commandList = [];
       }
   
       add(command) {
           this.commandList.push(command);
       }
   
       exec() {
           for (let i = 0, command; command = this.commandList[i++];) {
               command();
           }
       }
   }
   
   let macroCommand = new MacroCommand();
   
   macroCommand.add(command.closeDoor);
   macroCommand.add(command.openPC);
   macroCommand.add(command.openQQ);
   
   macroCommand.exec();
   ```

##### 13. 组合模式

1. 其实简单说来, 就类似于 树结构 的一种表现模式;

2. 文件扫描
   
   ```js
   class Folder {
       constructor(name) {
           this.name = name;
           this.files = [];
       }
   
       add(file) {
           this.files.push(file);
       }
   
       scan() {
           console.log('开始扫描文件夹: ' + this.name);
   
           for (let i = 0, file, files = this.files; file = files[i++];) {
               file.scan();
           }
       }
   }
   
   class File {
       constructor(name) {
           this.name = name
       }
   
       add() {
           throw new Error('文件下面不能添加文件!');
       }
   
       scan() {
           console.log('开始扫描文件夹: ' + this.name);
       }
   }
   
   const folder = new Folder('学习资料');
   const folder1 = new Folder('JavaScript');
   const folder2 = new Folder('jQuery');
   
   const file1 = new File('JavaScript1');
   const file2 = new File('JavaScript2');
   const file3 = new File('JavaScript3');
   
   folder1.add(file1);
   folder2.add(file2);
   
   folder.add(folder1);
   folder.add(folder2);
   folder.add(file3);
   
   const folder3 = new Folder('Node.js');
   const file4 = new File('深入浅出Node.js');
   folder3.add(file4);
   
   const file5 = new File('JavaScript 语言精髓与编程实战');
   
   folder.add(folder3);
   folder.add(file5);
   
   folder.scan();
   ```

##### 14. 模板方法模式

1. 定义: 一种只需使用继承的模式. 
   可以想象成: 先给定一个总体模板, 然后按照模板进行具体方法设计

2. 饮品案例
   
   ```js
   class Beverage {
       boilWater() {
           console.log('把水煮沸');
       }
   
       brew() {
           throw new Error('未重写brew方法');
       }
   
       pourInCup() {
           throw new Error('未重写pourInCup方法');
       }
   
       addCondiments() {
           throw new Error('未重写addCondiments方法');
       }
   
       customerWantsCondiments() {
           return true;
       }
   
       init() {
           this.boilWater();
           this.brew();
           this.pourInCup();
           if (this.customerWantsCondiments()) this.addCondiments();  // 钩子方法
       }
   }
   
     class Tea extends Beverage {
         brew() {
             console.log('用沸水冲泡茶');
         }
         pourInCup() {
             console.log('把茶倒进杯子');
         }
   
         customerWantsCondiments() {
             return window.confirm('请问需要加调料吗?');
         }
   
         addCondiments() {
             console.log('加糖和牛奶');
         }
   
     }
   
     const tea = new Tea();
     tea.init();
   
   ```
   
   

3. 钩子方法
   隔离变化, "钩子"到底挂不挂是可以由子类决定的.
   简单来说就是: 在定制模板时, 某方法不确定最终的结果, 于是新增一个钩子函数, 用于决定最终的结果.
   
   ```js
   customerWantsCondiments() {
      return true;
   }
   
    init() {
        if (this.customerWantsCondiments())
    this.addCondiments();  // 钩子方法
    }
   
   ```

4. 好莱坞原则
   
   允许底层组件将自己挂钩到高层组件中， 而高层组件会决定什么时候、以何种方式去使用这些底层组件。
   
   类似原则的运用：发布-订阅模式（观察着模式）， 回调函数

##### 15. 享元模式

1. (暂存)

##### 16.职责链模式

1. 


