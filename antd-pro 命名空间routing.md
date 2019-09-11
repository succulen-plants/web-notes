

### 为什么可以全局使用：命名空间routing

const data = yield select(({ routing }) => routing);

dva/src/index.js

import {
  routerMiddleware,
  routerReducer as routing,
} from 'react-router-redux';

import * as core from 'dva-core';
const createOpts = {
​    initialReducer: {
​      routing,
​    },}
const app = core.create(opts, createOpts);

dva-core:

var reducers = (0, _objectSpread2.default)({}, initialReducer);
 function createReducer() {
      ----
​    }
 var store = app._store = (0, _createStore.default)({
​      // eslint-disable-line
​      reducers: createReducer(),
​      }));
​      
