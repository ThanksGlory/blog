---
title: Promise (实现原理)
date: 2023-07-30 11:31:41
typora-root-url: ../
cover: /images/cover/promise.png
top_img: false
tags: JavaScript
categories:
  - JavaScript
  - Promise
---

**`Promise`** 是一个代理，它代表一个在创建 promise 时不一定已知的值。它允许你将处理程序与异步操作的最终成功值或失败原因关联起来。这使得异步方法可以像同步方法一样返回值：异步方法不会立即返回最终值，而是返回一个 *promise*，以便在将来的某个时间点提供该值，下面将用手写代码的方式体现Promise的原理：

```js
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

/**
 * @description Resolve定义
 * @typedef { (data:any) => void } Resolve
 */

/**
 * @description Reject定义
 * @typedef { (data:any) => void } Reject
 */

/**
 * @description State定义
 * @typedef {typeof PENDING | typeof FULFILLED | typeof REJECTED} State
 */

/**
 * @description Handler定义
 * @typedef Handler
 * @property {Resolve} Handler.onFulfilled
 * @property {Reject} Handler.onRejected
 * @property {Reject} Handler.resolve
 * @property {Reject} Handler.reject
 */

/**
 * 判断是否是promise
 * @param {any} data
 */
const isPromise = (data) => {
  if (data !== null && (typeof data === "object" | typeof data === "function")) {
    return typeof data.then === "function";
  }
  return false;
}

class MyPromise {

  /**
   * 处理完成执行函数
   * @param {any} value
   */
  static resolve(value) {
    return new MyPromise(resolve => {
      resolve(value);
    });
  }

  /**
   * 被拒绝执行函数
   * @param {any} reason
   */
  static reject = (reason) => {
    return new MyPromise((resolve, reject) => {
      reject(reason);
    });
  }

  /**
   * 当所有输入的 Promise 都被兑现时，
   * 返回的 Promise 也将被兑现（即使传入的是一个空的可迭代对象），
   * 并返回一个包含所有兑现值的数组。
   * 如果输入的任何 Promise 被拒绝，
   * 则返回的 Promise 将被拒绝，并带有第一个被拒绝的原因。
   * @param {MyPromise[]} promises
   * @memberof MyPromise
   */
  static all = (promises = []) => {
    let results = [];
    let count = 0;
    let len = promises.length;
    return new MyPromise((resolve, reject) => {
      promises.forEach((val, index) => {
        val.then(
          result => {
            results[index] = result;
            count = count + 1;
            if (count === len) resolve(results);
          },
          error => {
            reject(error);
          }
        );
      });
    });
  }

  /**
   * 所有输入的 Promise 都已敲定时（包括传入空的可迭代对象时），
   * 返回的 Promise 将被兑现，并带有描述每个 Promise 结果的对象数组。
   * @param {MyPromise[]} promises
   * @returns MyPromise
   */
  static allSettled = (promises = []) => {
    let statuses = [];
    let len = promises.length - 1;
    return new MyPromise(resolve => {
      promises.forEach((val, index) => {
        val.then(
          () => {
            statuses[index] = {
              status: FULFILLED
            };
            if (index === len) resolve(statuses);
          },
          () => {
            statuses[index] = {
              status: REJECTED
            };
            if (index === len) resolve(statuses);
          }
        );
      });
    });
  }

  /**
   * 当输入的任何一个 Promise 兑现时，这个返回的 Promise 将会兑现，
   * 并返回第一个兑现的值。
   * 当所有输入 Promise 都被拒绝（包括传递了空的可迭代对象）时，
   * 它会以一个包含拒绝原因数组的 AggregateError 拒绝。
   * @param {MyPromise[]} promises
   * @returns MyPromise
   */
  static any = (promises = []) => {
    return new MyPromise((resolve, reject) => {
      promises = Array.isArray(promises) ? promises : [];
      let len = promises.length;
      // 用于收集所有 reject 
      let errs = [];
      // 如果传入的是一个空数组，那么就直接返回 AggregateError
      if (len === 0) return reject(new AggregateError('All promises were rejected'));
      promises.forEach((promise) => {
        promise.then(value => {
          resolve(value);
        }, err => {
          len--;
          errs.push(err);
          if (len === 0) {
            reject(new AggregateError(errs));
          }
        })
      })
    });
  }

  /**
   * 这个返回的 promise 会随着第一个 promise 的敲定而敲定。
   * @param {MyPromise[]} promises
   * @memberof MyPromise
   */
  static race = (promises = []) => {
    return new MyPromise((resolve, reject) => {
      promises.forEach(item => {
        if (isPromise(item)) {
          item.then(resolve, reject);
        } else {
          resolve(item);
        }
      });
    });
  }

  /**
   * Promise状态
   * @type {State}
   * @memberof MyPromise
   */
  #state = PENDING;

  /**
   * 存储promise结果
   * @type {any}
   * @memberof MyPromise
   */
  #result = undefined;

  /**
   * 存储then方法中的参数
   * @type {Handler[]}
   */
  #handlers = [];

  /**
   * 创建一个MyPromise的实例对象
   * @param { (resolve: Resolve, reject: Reject) => void } executor
   * @memberof MyPromise
   */
  constructor(executor) {
    try {
      executor(this.#resolve, this.#reject);
    } catch (err) {
      this.#reject(err);
    }
  }

  /**
   * @description 执行成功
   * @param {any} result
   */
  #resolve = (result) => {
    this.#changeState(FULFILLED, result);
  }

  /**
   * @description 执行失败
   * @param {any} reason
   */
  #reject = (reason) => {
    this.#changeState(REJECTED, reason);
  }

  /**
   * @description 修改状态
   * @param {State} state 状态
   * @param {any} result 结果
   */
  #changeState = (state, result) => {
    if (this.#state !== PENDING) return;
    this.#state = state;
    this.#result = result;
    this.#run();
  }

  /**
   * 将函数添加到微任务里面
   * @param {()=>void} func
   */
  #runMicroTask = (func) => {
    if (typeof process === "object" && typeof process.nextTick === "function") {
      process.nextTick(func);
    } else if (typeof queueMicrotask === "function") {
      queueMicrotask(func);
    }
  }

  /**
   * @description 抽离run中的重复代码
   * @param {(value:any)=>MyPromise|undefined} callback
   * @param {Resolve} resolve
   * @param {Reject} reject
   */
  #runOne = (callback, resolve, reject) => {
    this.#runMicroTask(() => {
      if (typeof callback !== "function") {
        let settled = this.#state === FULFILLED ? resolve : reject;
        settled(this.#result);
        return;
      }

      try {
        const data = callback(this.#result);
        if (isPromise(data)) {
          data.then(resolve, reject);
        } else {
          resolve(data);
        }
      } catch (error) {
        reject(error);
      }
    });
  }

  /**
   * @description 执行then存储的回调方法
   */
  #run = () => {
    if (this.#state === PENDING) return;

    while (this.#handlers.length) {
      /**
       * @type {Handler}
       */
      const handler = this.#handlers.shift();
      const { onFulfilled, onRejected, resolve, reject } = handler;
      if (this.#state === FULFILLED) {
        this.#runOne(onFulfilled, resolve, reject);
      } else if (this.#state === REJECTED) {
        this.#runOne(onRejected, resolve, reject);
      }
    }
  }

  /**
   * @description Promise then 方法
   * @param {Resolve} onFulfilled
   * @param {Reject} onRejected
   * @memberof MyPromise
   */
  then = (onFulfilled, onRejected) => {
    return new MyPromise((resolve, reject) => {
      this.#handlers.push({
        onFulfilled,
        onRejected,
        resolve,
        reject,
      });
      this.#run();
    });
  }

  /**
   * 用来捕获 promise 的异常，就相当于一个没有成功的 then
   * @param {(value:any)=>void} errCallback 
   */
  catch = (errCallback) => {
    return this.then(null, errCallback);
  }

  /**
   * finally 表示不是最终的意思，而是无论如何都会执行的意思。
   * 如果返回一个 promise 会等待这个 promise 也执行完毕。
   * 如果返回的是成功的 promise，会采用上一次的结果；
   * 如果返回的是失败的 promise，会用这个失败的结果，传到 catch 中。
   * @param {(value:any)=>void} callback 
   */
  finally = (callback) => {
    return this.then(
      (value) => {
        return MyPromise.resolve(callback()).then(() => value)
      },
      (reason) => {
        return MyPromise.resolve(callback()).then(() => { throw reason })
      }
    );
  }
}
```

