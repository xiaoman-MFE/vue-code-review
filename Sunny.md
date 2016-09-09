
>ecb125d commit@ 07/09/2014

`util.js` 定义了 第一个 mixin 工具集 方便原型的快速绑定

```
// utils.js
  mixin = (target, mixin) => {
      // 原型里有该方法不同就赋值
    }
```

> b5bfc59 commit@ 07/09/2014 1:35pm

`emitter.js`: 事件发射器
  - `on(event, fn)` : 动态把 fn 加入对应event 的数组对了 _cbs =》callbacks
  - `once(event, fn)` : 重新包裹fn 注入 on； 触发时 会先off 掉该callback 然后 执行方法
  - `off(event, fn)` : 没有参数则返回 只有event 删掉所有的 callbacks ; 2个参数都有 找到对应的从数组splice
  - `emit(event, a, b, c)` : 拿到对应的cbs 去依次执行 FIFO
  - `applyEmit(event)` : 与 emit 类似 差别是使用 apply 执行callback 不限制 arguments 数量
  注意 `call` `apply` 使用差别
  `call(obj[,arg1[,arg2 ...]])` : 参数固定 一个个传进去
  `apply(obj[,args])` : 参数部分直接是数组 不限个数

`observer.js` : 数据观察 *第一个核心部分?*
  -`observe(obj)` : 已经有$observer 则返回 跟进类型 分别 observe *Array* *Object*
注意连等使用 A=B=C => B=C, A=B 
注意案例
```
var a = {n:1}; // 原引用 {n:1}}
a.x = a = {n:2}; 新引用 {n:2} 原引用 {n:1,x:{n:2}} 没有被引用垃圾回收掉
console.log(a.x); // 输出 undefined
```

> 0ad7f54 commit@ 07/10/2014 7:08am
`observer.js` 
  - 定义了 Array 的观测 重写 push pop ..原生方法每次操作后 会 link unlink 相应元素
  - 针对Object 用walk 给每一个key加上ob 观测
  
>branch 0.10 871ed91 commit@ 07/29/2013
最初的逻辑 
给所有 {{}} 做一次带attr标记的替换 
然后利用这些标记建立 bindings[variable].els 管理数组
defineProperty set时候 将值赋给textContent





