# 运用了哪些设计模式

Connerstone 作为一款优秀的医学影像js库，应用了大量的javascript语言组织基础库的技巧。

## 事件机制

事件机制是Connerstone传递信息的最关键的机制。通过发布订阅的方式，Cornerstone可以和上层应用或工具进行跨组件的沟通，自身模块之间也可以迅速传递信息。根据不同的场景，设置不同的事件主体（有全局事件，特定元素的事件）。

事件机制的好处有：

* 一对多，所有依赖的对象都可以感知变化
* 任何一个流程都可以被消费方进行捕获
* 接口的扩展性也非常好
* 模块之间可以结耦

``` 
// 代码略有改动，只展示了核心原理，具体步骤忽略
const EVENTS = {
  NEW_IMAGE: 'cornerstonenewimage',
  // ...
};
class EventTarget {
  constructor () {
    this.listeners = {};
    this.namespaces = {};
  }
  addEventNamespaceListener (type, callback) {
    // ...
  }
  removeEventNamespaceListener (type) {
   // ...
  }
  addEventListener (type, callback) {
    // ...
  }
  removeEventListener (type, callback) {
    // ...
  }
  dispatchEvent (event) {
    // ...
  }
}
export const events = new EventTarget();
```

