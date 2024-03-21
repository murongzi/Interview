模拟promise

```javascript


class PromiseMock {
  status = 'pending';
  resolveCallback = [];
  rejectCallback = [];

  static resolve() {
    return new PromiseMock(function(resolve) {
      resolve();
    });
  }

  static reject() {
    return new PromiseMock(function(resolve, reject) {
      reject();
    });
  }

  static all() {
    const promiseList = arguments

    return new PromiseMock(function(resolve, reject) {
      const result = []
      function fnResolveCallback(value, i) {
        result.push(value);

        if (result.length === promiseList.length) {
          resolve(result);
        }
      }

      promiseList.forEach((arg, i) => {
        arg.then(function(value) {
          fnResolveCallback(value, i);
        }, reject)
      })
    });
  }

  constructor(fn) {
    let that = this;

    fn.call(undefined, function() {
      that.resolve();
    }, function() {
      that.reject();
    });
  }

  resolve() {
    const that = this;
    that.status = "fulfilled";

    while(that.resolveCallback.length) {
      that.resolveCallback.shift().call(that);
    }
  }

  reject() {
    const that = this;
    that.status = "rejected";

    let result;
    while(that.rejectCallback.length) {
      result = that.rejectCallback.shift().call(that, result);
    }
  }

  then(resolveFn, rejectFn) {
    if (this.status === 'fulfilled') {
      resolveFn?.();
    } else if (this.status === 'rejected') {
      rejectFn?.();
    } else {
      resolveFn && this.resolveCallback.push(resolveFn);
      rejectFn && this.rejectCallback.push(rejectFn);
    }

    return this;
  }

  finally(fn) {
    this.then(fn, fn);

    return this;
  }

  catch(fn) {
    this.then(undefined, fn);
  }

}

const pMock = new PromiseMock(function(resolve, reject) {
  setTimeout(() => {
    resolve();
    console.log('==== resolve', this);
  }, 1000)
}).then(() => {
  console.log("=== resolve then");
});

pMock.finally(function(){
  console.log('=====sss');
});

setTimeout(function() {
  pMock.then(function() {
    console.log('==== delay');
  })
}, 3000)


```

