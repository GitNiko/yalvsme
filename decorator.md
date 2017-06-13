# decorator的5种方式

decorator编程技巧经常能被使用到。特意去总体看了下，顺便记录了5种decorator的方式。

如果英语好可以看看以下的链接。

* [The decorator pattern in JavaScript](http://nickmeldrum.com/blog/decorators-in-javascript-using-monkey-patching-closures-prototypes-proxies-and-middleware?utm_source=javascriptweekly&utm_medium=email)

## closures

闭包的方式存储修饰参数，返回新的对象，这个函数把修饰参数传送给原来对象的方法并返回结果。

这是一种wrap的过程。

## monkey patching

直接替换原来的对象的方法。

这是一种替换的过程。

## Prototypal inheritance

直接改原型链上的方法。

这一种继承的过程。

## Proxy

ES6的新特性，[代理模式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

这是一种代理的过程。

## Middleware

```
// apply our fns
[fn1, fn2, fn3]
// reverse
[fn3, fn2, fn1]
// for each concat
finalFn = fn1(fn2(fn3(originFn)))
// we use finalFn
finalFn(3) = fn1(fn2(fn3(originFn(3))))
```

## Middleware
重型装饰器

```js
function myComponentFactory() {
    let suffix = ''
    const instance = {
        setSuffix: suff => suffix = suff,
        printValue: value => console.log(`value is ${value + suffix}`),
        addDecorators: decorators => {
            let printValue = instance.printValue
            decorators.slice().reverse().forEach(decorator => printValue = decorator(printValue))
            // instance.printValue = _.compose(...decorators)(printValue);
            instance.printValue = printValue
        }
    }
    return instance
}

function toLowerDecorator(inner) {
    return value => inner(value.toLowerCase())
}

function validatorDecorator(inner) {
    return value => {
        const isValid = ~value.indexOf('My')

        setTimeout(() => {
            if (isValid) inner(value)
            else console.log('not valid man...')
        }, 500)
    }
}

const component = myComponentFactory()
component.addDecorators([toLowerDecorator, validatorDecorator])
component.setSuffix('!')
component.printValue('My Value')
component.printValue('Invalid Value')
```
