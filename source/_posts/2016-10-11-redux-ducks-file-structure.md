---
title: redux 'ducks' 文件组织结构
tags:
  - React
  - Redux
categories:
  - 前端
  - React
date: 2016-10-10 09:16:15
---

> https://github.com/erikras/ducks-modular-redux

传统上，一个react/redux项目会有类似以下的目录

```
|_ containers
|_ components
|_ reducers
|_ actions
|_ constants
|_ ...
```

然而对于不太复杂的项目，很多情况下我们只需要用到一两个reducer和action(creator)，上面的目录结构由于将逻辑上的功能分散使得import有些繁琐。以下按模块划分的`Ducks`目录结构更直观

<!-- more -->

```javascript widgets.js
// Actions
const LOAD   = 'my-app/widgets/LOAD';
const CREATE = 'my-app/widgets/CREATE';
const UPDATE = 'my-app/widgets/UPDATE';
const REMOVE = 'my-app/widgets/REMOVE';

// Reducer
export default function reducer(state = {}, action = {}) {
  switch (action.type) {
    // do reducer stuff
    default: return state;
  }
}

// Action Creators
export function loadWidgets() {
  return { type: LOAD };
}

export function createWidget(widget) {
  return { type: CREATE, widget };
}

export function updateWidget(widget) {
  return { type: UPDATE, widget };
}

export function removeWidget(widget) {
  return { type: REMOVE, widget };
}
```

**约定**

一个所谓*模块*，

1. 必须`export default`一个叫`reducer()`的函数(也就是我们的reducer)
2. 必须`export`它的action creators函数
3. 以类似`npm-module-or-app/reducer/ACTION_TYPE`的方式定义他的**action type**
4. 如果必要，也`export`它的**action type**