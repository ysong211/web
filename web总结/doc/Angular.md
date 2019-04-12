# 1.使用Angular，和使用Angularjs 1相比，有什么优势？
1. Angular 是一个平台，不仅是一种语言
1. 更好的速度和性能
1. 更简单的依赖注入
1. 模块化，跨平台
1. 具备ES6和Typescript的好处。
1. 灵活的路由，具备延迟加载功能
1. 更容易学习
# 2. 如何优化Angular应用程序来获得更好的性能？
1. 考虑AOT编译。
1. 确保应用程序已经经过了捆绑，uglify和tree shaking。
1. 确保应用程序不存在不必要的import语句。
1. 确保应用中已经移除了不使用的第三方库。
1. 所有dependencies 和dev-dependencies都是明确分离的。
1. 如果应用程序较大时，我会考虑延迟加载而不是完全捆绑的应用程序。
#  3. 什么是Shadow DOM？它如何帮助Angular更好地执行？
Shadow DOM是HTML规范的一部分，它允许开发人员封装自己的HTML标记，CSS样式和JavaScript。Shadow DOM以及其它一些技术，使开发人员能够像<audio>标签一样构建自己的一级标签，Web组件和API。总的来说，这些新的标签和API被称为Web组件。Shadow DOM通过提供了更好的关注分离，通过其它的HTML DOM元素实现了更少的样式与脚本的冲突。
因为shadow DOM本质上是静态的，同时也是开发人员无法访问的，所以它是一个很好的候选对象。因为它缓存的DOM将在浏览器中呈现得更快，并提供更好的性能。此外，还可以相对很好地管理shadow DOM，同时检测Angular应用的改变，并且可以有效地管理视图的重新绘制。
# 4. 什么是AOT编译？它有什么优缺点？
AOT编译代表的是Ahead Of Time编译，其中Angular编译器在构建时，会将Angular组件和模板编译为本机JavaScript和HTML。编译好的HTML和JavaScript将会部署到Web服务器，以便浏览器可以节省编译和渲染时间。
- 优点：
    - 更快的下载：由于应用程序已经编译，许多Angular编译器相关库就不再需要捆绑，应用程序包变得更小，所以该应用程序可以更快地下载。
    - 更少的Http请求数：如果应用程序没有捆绑来支持延迟加载（或任何原因），对于每个关联的HTML和CSS，都会有一个单独的服务器请求。但是预编译的应用程序会将所有模板和样式与组件对齐，因此到服务器的Http请求数量会更少。
    - 更快的渲染：如果应用程序不是AOT编译，那么应用程序完全加载时，编译过程会发生在浏览器中。这需要等待下载所有必需的组件，然后等待编译器花费时间来编译应用程序。使用AOT编译，就能实现优化。
    - 在构建时检测错误：由于预先编译，可以检测到许多编译时错误，能够为应用程序提供更好的稳定性。

- 缺点：
    - 仅适用于HTML和CSS，其它文件类型需要前面的构建步骤
    - 没有watch模式，必须手动完成（bin / ngc-watch.js）并编译所有文件
    - 需要维护AOT版本的bootstrap文件（使用cli等工具时不需要）
    - 在编译之前，需要清理步骤
    
# 5.Observables和Promises的核心区别是什么？
堆栈溢出就是一个区别： 
当异步操作完成或失败时，Promise会处理一个单个事件。
Observable类似于（在许多语言中的）Stream，当每个事件调用回调函数时，允许传递零个或多个事件。通常Observable比Promise更受欢迎，因为它不但提供了Promise特性，还提供了其它特性。使用Observable可以处理0,1或多个事件。你可以在每种情况下使用相同的API。Observable是可取消的，这相比于Promise也具有优势。如果服务器的HTTP请求结果或其它一些异步操作不再需要，则Observable的订阅者可以取消订阅，而Promise将最终调用成功或失败的回调，即使你不需要通知或其提供的结果。Observable提供像map，forEach，reduce之类的类似于数组的运算符，还有强大的运算符，如retry（）或replay（）等，使用起来是相当方便的。

Promises vs Observables

    - Promises：
        - 返回单个值
        - 不可取消
    - Observables： 
        - 可以使用多个值
        - 可取消
        - 支持map，filter，reduce和类似的操作符
        - ES 2016提议的功能
        - 使用反应式扩展（RxJS）
        - 根据时间的变化，数组成员可以异步获取