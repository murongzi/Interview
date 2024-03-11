## 1: 原型链式继承

```javascript

function Parent() {
    this.name = 'parent';
}

Parent.prototype.sayName = function () {
    console.log(this.name);
};

function Child() {
    this.name = 'child';
}

Child.prototype = new Parent();

const child = new Child();

child.sayName();

console.log('====child instanceof Parent', child instanceof Parent);
console.log('====child instanceof Child', child instanceof Child);

```

缺点：

父类的实例属性会被挂到子类的原型上，

## 2：借用父类的构造函数
```javascript
function Parent() {
    this.name = 'parent';
    console.log('parent constructor')
}

Parent.prototype.sayName = function () {
    console.log(this.name);
};

function Child() {
    Parent.apply(this, arguments);

    this.name = 'child';
    console.log('child constructor')
}

const child = new Child();

child.sayName();

```
优点：
可以通过父类构造实例属性

缺点：
无法共享使用父类的原型属性


## 3：组合继承
```javascript
function Parent() {
    this.name = 'parent';
    console.log('parent constructor')
}

Parent.prototype.sayName = function () {
    console.log(this.name);
};

function Child() {
    Parent.apply(this, arguments);

    this.name = 'child';
    console.log('child constructor')
}

Child.prototype = new Parent();

Child.prototype.constructor = Child;

const child = new Child();

child.sayName();

```

缺点：
Parent构造函数会被调用两次
父构造函数上的属性仍然被挂在到子类的原型上，而且子类上也拥有了父类的实例属性


## 4：原型式继承
```javascript
const prototype = {
    name: '123',
    detail: [1,2,34],
    sayName() {
        console.log(this.name);
    }
}

function createPerson(prototype) {
    function f() {}

    f.prototype = prototype;

    return new f();
}

const child = createPerson(prototype);

child.sayName();

```
缺点：应用类型属性会被共享

## 5：寄生式继承
```javascript

function createFactory(prototype) {
    var obj = Object.create(prototype);

    obj.sayName = () => {
        console.log('sayName');
    }

    return obj;
}

const obj = createFactory({
    name: 'John',
    age: 20
});

```
缺点：

方法无法被共享


## 6：寄生组合式继承
```javascript
function create(prototype) {
    const fn = function () {}

    fn.prototype = prototype

    return new fn();
}

const createFactory = (child, parent) => {
    const _prototype = create(parent.prototype);

    child.prototype = _prototype;
    child.prototype.constructor = child;
}

function Parent() {
    this.name = 'parent'
}

Parent.prototype.sayName = function () {
    console.log('parent name sayName', this.name)
}

function Child() {
    Parent.apply(this, arguments);
}

createFactory(Child, Parent);

const child = new Child();

child.sayName();

```