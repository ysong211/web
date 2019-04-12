
# Vue的MVVM
```
MVVM全称是Model-View-ViewModel,Vue是以数据驱动的,一旦dom创建,数据更新dom也就跟着更新
1、M就是Model模型层，存的一个数据对象。
2、V就是View视图层，所有的html节点在这一层。
3、VM就是ViewModel，它通过data属性连接Model模型层，通过el属性连接View视图层
```
###### MVP
![MVP](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015020109.png)
```
1. 各部分之间的通信，都是双向的。

2. View 与 Model 不发生联系，都通过 Presenter 传递。

3. View 非常薄，不部署任何业务逻辑，称为"被动视图"（Passive View），即没有任何主动性，而 Presenter非常厚，所有逻辑都部署在那里。
```

###### MVVM
![MVVM](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015020110.png)
```
类似MVP,Presenter 改名为 ViewModel
```
###### MVC
![MVC](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015020105.png)
```
1. View 传送指令到 Controller
2. Controller 完成业务逻辑后，要求 Model 改变状态
3. Model 将新的数据发送到 View，用户得到反馈
```



# keep-live
```
把切换出去的组件保留在缓存中，可以保留组件的状态或者避免重新渲染
```
# 组件之间通讯
```
1.父子组件
prop  $emit
同级组件之间通讯vuex
2.兄弟组件
eventBus


```
# vue生命周期的理解

创建前/后，载入前/后，更新前/后，销毁前/后
它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。

- beforeCreated() 在实例创建之间执行，数据未加载状态
- created() 在实例创建、数据加载后，能初始化数据，dom渲染之前执行
- beforeMount() 虚拟dom已创建完成，在数据渲染前最后一次更改数据
- mounted() 页面、数据渲染完成，真实dom挂载完成
- beforeUpadate() 重新渲染之前触发
- updated() 数据已经更改完成，dom 也重新 render 完成,更改数据会陷入死循环
- beforeDestory() 和 destoryed() 前者是销毁前执行（实例仍然完全可用），后者则是销毁后执行

# vue响应式原理

## Object.defineProperty
首先我们想到的是Object.defineProperty,这是es5新增的一个api,它可以允许我们为对象的属性来设定getter和setter,从而我们可以劫持用户对对象属性的取值和赋值。比如以下代码:
```cgo
const obj = {
};

let val = 'cjg';
Object.defineProperty(obj, 'name', {
  get() {
    console.log('劫持了你的取值操作啦');
    return val;
  },
  set(newVal) {
    console.log('劫持了你的赋值操作啦');
    val = newVal;
  }
});

console.log(obj.name);
obj.name = 'cwc';
console.log(obj.name);
```
我们通过Object.defineProperty劫持了obj[name]的取值和赋值操作，因此我们就可以在这里做一些手脚啦，比如说，我们可以在obj[name]被赋值的时候触发更新页面操作。
## 发布订阅模式
发布订阅模式是设计模式中比较常见的一种，其中有两个角色：发布者和订阅者。多个订阅者可以向同一发布者订阅一个事件，当事件发生的时候，发布者通知所有订阅该事件的订阅者。我们来看一个例子了解下。
```cgo
class Dep {
  constructor() {
    this.subs = [];
  }
  // 增加订阅者
  addSub(sub) {
    if (this.subs.indexOf(sub) < 0) {
      this.subs.push(sub);
    }
  }
  // 通知订阅者
  notify() {
    this.subs.forEach((sub) => {
      sub.update();
    })
  }
}

const dep = new Dep();

const sub = {
  update() {
    console.log('sub1 update')
  }
}

const sub1 = {
  update() {
    console.log('sub2 update');
  }
}

dep.addSub(sub);
dep.addSub(sub1);
dep.notify(); //
```

- vue.js首先通过Object.defineProperty来对要监听的数据进行getter和setter劫持，当数据的属性被赋值/取值的时候，vue.js就可以察觉到并做相应的处理。
- 通过订阅发布模式，我们可以为对象的每个属性都创建一个发布者，当有其他订阅者依赖于这个属性的时候，则将订阅者加入到发布者的队列中。利用Object.defineProperty的数据劫持，在属性的setter调用的时候，该属性的发布者通知所有订阅者更新内容。


# vuex几大属性
```
有五种，分别是 State、 Getter、Mutation 、Action、 Module
```
# state
```
1、Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于与一般Vue对象里面的data
2、state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
3、它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中
```
# getter
```
1、getters 可以对State进行计算操作，它就是Store的计算属性
2、 虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
3、 如果一个状态只在一个组件内使用，是可以不用getters
```
# mutation
```
sequenceDiagram
A->>B: How are you?
B->>A: Great!
```
```
Action 类似于 mutation，不同在于：
 - Action 提交的是 mutation，而不是直接变更状态。
 - Action 可以包含任意异步操作
```
# vue优点
```
低耦合。视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的"View"上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
可重用性。你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。
独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xml代码。
```
# vue常用指令
```
v-if v-show v-bind(:) v-for v-model  v-text v-html v-on(@)
```

# nextTick

在下次dom更新循环结束之后执行延迟回调，可用于获取更新后的dom状态


- 新版本中默认是mincrotasks, v-on中会使用macrotasks


- macrotasks任务的实现:

    - setImmediate / MessageChannel / setTimeout


# 数据响应(数据劫持)

看完生命周期后，里面的watcher等内容其实是数据响应中的一部分。数据响应的实现由两部分构成: 观察者( watcher ) 和 依赖收集器( Dep )，其核心是 defineProperty这个方法，它可以 重写属性的 get 与 set 方法，从而完成监听数据的改变。

- Observe (观察者)观察 props 与 state
    - 遍历 props 与 state，对每个属性创建独立的监听器( watcher )


- 使用 defineProperty 重写每个属性的 get/set(defineReactive）

    - get: 收集依赖
        - Dep.depend()
            - watcher.addDep()
    - set: 派发更新
        - Dep.notify()
        - watcher.update()
        - queenWatcher()
        - nextTick
        - flushScheduleQueue
        - watcher.run()
        - updateComponent()

```
let data = {a: 1}
// 数据响应性
observe(data)

// 初始化观察者
new Watcher(data, 'name', updateComponent)
data.a = 2

// 简单表示用于数据更新后的操作
function updateComponent() {
    vm._update() // patchs
}

// 监视对象
function observe(obj) {
	 // 遍历对象，使用 get/set 重新定义对象的每个属性值
    Object.keys(obj).map(key => {
        defineReactive(obj, key, obj[key])
    })
}

function defineReactive(obj, k, v) {
    // 递归子属性
    if (type(v) == 'object') observe(v)
    
    // 新建依赖收集器
    let dep = new Dep()
    // 定义get/set
    Object.defineProperty(obj, k, {
        enumerable: true,
        configurable: true,
        get: function reactiveGetter() {
        	  // 当有获取该属性时，证明依赖于该对象，因此被添加进收集器中
            if (Dep.target) {
                dep.addSub(Dep.target)
            }
            return v
        },
        // 重新设置值时，触发收集器的通知机制
        set: function reactiveSetter(nV) {
            v = nV
            dep.nofify()
        },
    })
}

// 依赖收集器
class Dep {
    constructor() {
        this.subs = []
    }
    addSub(sub) {
        this.subs.push(sub)
    }
    notify() {
        this.subs.map(sub => {
            sub.update()
        })
    }
}

Dep.target = null

// 观察者
class Watcher {
    constructor(obj, key, cb) {
        Dep.target = this
        this.cb = cb
        this.obj = obj
        this.key = key
        this.value = obj[key]
        Dep.target = null
    }
    addDep(Dep) {
        Dep.addSub(this)
    }
    update() {
        this.value = this.obj[this.key]
        this.cb(this.value)
    }
    before() {
        callHook('beforeUpdate')
    }
}

```

# virtual dom 原理实现

- 创建 dom 树


- 树的diff，同层对比，输出patchs(listDiff/diffChildren/diffProps)
    - 没有新的节点，返回
    - 
    - 新的节点tagName与key不变， 对比props，继续递归遍历子树
        - 对比属性(对比新旧属性列表):
            - 旧属性是否存在与新属性列表中
            - 都存在的是否有变化
            - 是否出现旧列表中没有的新属性
            
        - tagName和key值变化了，则直接替换成新节点



- 渲染差异

    - 遍历patchs， 把需要更改的节点取出来
    - 局部更新dom

```
// diff算法的实现
function diff(oldTree, newTree) {
	 // 差异收集
    let pathchs = {}
    dfs(oldTree, newTree, 0, pathchs)
    return pathchs
}

function dfs(oldNode, newNode, index, pathchs) {
    let curPathchs = []
    if (newNode) {
        // 当新旧节点的 tagName 和 key 值完全一致时
        if (oldNode.tagName === newNode.tagName && oldNode.key === newNode.key) {
        	  // 继续比对属性差异
            let props = diffProps(oldNode.props, newNode.props)
            curPathchs.push({ type: 'changeProps', props })
            // 递归进入下一层级的比较
            diffChildrens(oldNode.children, newNode.children, index, pathchs)
        } else {
        	  // 当 tagName 或者 key 修改了后，表示已经是全新节点，无需再比
            curPathchs.push({ type: 'replaceNode', node: newNode })
        }
    }

	 // 构建出整颗差异树
    if (curPathchs.length) {
    		if(pathchs[index]){
    			pathchs[index] = pathchs[index].concat(curPathchs)
    		} else {
    			pathchs[index] = curPathchs
    		}
    }
}

// 属性对比实现
function diffProps(oldProps, newProps) {
    let propsPathchs = []
    // 遍历新旧属性列表
    // 查找删除项
    // 查找修改项
    // 查找新增项
    forin(olaProps, (k, v) => {
        if (!newProps.hasOwnProperty(k)) {
            propsPathchs.push({ type: 'remove', prop: k })
        } else {
            if (v !== newProps[k]) {
                propsPathchs.push({ type: 'change', prop: k , value: newProps[k] })
            }
        }
    })
    forin(newProps, (k, v) => {
        if (!oldProps.hasOwnProperty(k)) {
            propsPathchs.push({ type: 'add', prop: k, value: v })
        }
    })
    return propsPathchs
}

// 对比子级差异
function diffChildrens(oldChild, newChild, index, pathchs) {
		// 标记子级的删除/新增/移动
    let { change, list } = diffList(oldChild, newChild, index, pathchs)
    if (change.length) {
        if (pathchs[index]) {
            pathchs[index] = pathchs[index].concat(change)
        } else {
            pathchs[index] = change
        }
    }

	 // 根据 key 获取原本匹配的节点，进一步递归从头开始对比
    oldChild.map((item, i) => {
        let keyIndex = list.indexOf(item.key)
        if (keyIndex) {
            let node = newChild[keyIndex]
            // 进一步递归对比
            dfs(item, node, index, pathchs)
        }
    })
}

// 列表对比，主要也是根据 key 值查找匹配项
// 对比出新旧列表的新增/删除/移动
function diffList(oldList, newList, index, pathchs) {
    let change = []
    let list = []
    const newKeys = getKey(newList)
    oldList.map(v => {
        if (newKeys.indexOf(v.key) > -1) {
            list.push(v.key)
        } else {
            list.push(null)
        }
    })

    // 标记删除
    for (let i = list.length - 1; i>= 0; i--) {
        if (!list[i]) {
            list.splice(i, 1)
            change.push({ type: 'remove', index: i })
        }
    }

    // 标记新增和移动
    newList.map((item, i) => {
        const key = item.key
        const index = list.indexOf(key)
        if (index === -1 || key == null) {
            // 新增
            change.push({ type: 'add', node: item, index: i })
            list.splice(i, 0, key)
        } else {
            // 移动
            if (index !== i) {
                change.push({
                    type: 'move',
                    form: index,
                    to: i,
                })
                move(list, index, i)
            }
        }
    })

    return { change, list }
}

```
# Proxy 相比于 defineProperty 的优势
- 数组变化也能监听到
- 不需要深度遍历监听

# 写 React / Vue 项目时为什么要在组件中写 key，其作用是什么
```
key的作用是为了在diff算法执行时更快的找到对应的节点，提高diff速度
```


