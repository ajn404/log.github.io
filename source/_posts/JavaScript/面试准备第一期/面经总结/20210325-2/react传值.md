---
title: react传值问题
---

###  react传值方法

1. 父组件->子组件，props

   缺点：嵌套深，交流成本变高

2. 子组件->父组件，传入回调函数给props

   缺点同上

3. 兄弟组件之间通信，[react状态提升](https://zh-hans.reactjs.org/docs/lifting-state-up.html)

4. useContext

5. 使用观察者模式，引入EventEmitter或者[postal.js](https://www.bootcdn.cn/postal.js/)

6. 使用状态管理库，MobX或者Redux

### React状态提升实例

<iframe height="400" style="width: 100%;" scrolling="no" title="ExZVOKq" src="https://codepen.io/ajn404/embed/ExZVOKq?height=265&theme-id=light&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ajn404/pen/ExZVOKq'>ExZVOKq</a> by ajn404
  (<a href='https://codepen.io/ajn404'>@ajn404</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

参考

- https://www.cnblogs.com/yoable/p/8017354.html
- 《React进阶之路》