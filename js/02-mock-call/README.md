
```javascript

Function.prototype.call2 = function (ct) {
    ct = ct || window;
    console.log(this);

    let str = [];

    for (var i = 1, l = arguments.length; i < l; i++) {
        str.push('arguments[' + i + ']');
    }

    ct.fn = this;

    var result = eval('ct.fn(' + str.join(',') + ')');

    delete ct.fn;

    return result
}

function aa() {
    console.log('====aaaa', this, arguments);
    return 123;
}

aa.call2({
    a: 'test'
}, "person", "aas");

```
