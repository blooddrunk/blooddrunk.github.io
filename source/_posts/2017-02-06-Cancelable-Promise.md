---
title: Cancelable Promise
date: 2017-02-06 23:49:54
tags:
  - ES6
  - React
categories:
  - 前端
  - Javascript
---

使用React时难免会需要在异步操作返回时调用`setState()`，如果此时该Component已经Unmount, React会抛出类似下面的错误：

> "Warning: setState(...): Can only update a mounted or mounting component. This usually means you called setState() on an unmounted component. This is a no-op. Please check the code for the ContactPage component."

解决这个问题最简单的方法就是在Lifecyle事件中手动设置并在调用`setState()`前检查某个标记位：

```javascript inside a component
  componentDidMount() {
    this._isMounted = true
  }

  componentWillUnmount() {
    this._isMounted = false
  }

  // 调用setState时
  if(this._isMounted) {
    this.setState(...)
  }
```

介于现在普遍都在使用**Fetch API**和**Promise**，更好的办法是使用可被Cancel的Promise，一个简单的实现如下：

<!-- More -->

```javascript cancelable promise
const makeCancelable = (promise) => {
  let hasCanceled_ = false

  const wrappedPromise = new Promise((resolve, reject) => {
    promise.then((val) =>
      hasCanceled_ ? reject({isCanceled: true}) : resolve(val)
    )
    promise.catch((error) =>
      hasCanceled_ ? reject({isCanceled: true}) : reject(error)
    )
  });

  return {
    promise: wrappedPromise,
    cancel() {
      hasCanceled_ = true;
    },
  }
}
```

通过在原来返回Promise的调用外包裹一层*Higher Order Function*(虽然实际上返回的是一个新的对象), 然后在必要时(`componentWillUnmount`)取消掉:

```javascript 
const cancelablePromise = makeCancelable(
  new Promise(r => component.setState({...}}))
);

cancelablePromise
  .promise
  .then(() => console.log('resolved'))
  .catch((reason) => console.log('isCanceled', reason.isCanceled));

cancelablePromise.cancel(); // Cancel the promise
```

{% link Eric Elliott https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261#.l7d37jad3 %}大神给出了更完善的实现: <https://github.com/ericelliott/speculation>

