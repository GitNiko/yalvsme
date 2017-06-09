# 关键字解释

这里放了些关键字解释

## Side effect
>
In computer science, a function or expression is said to have a side effect if it modifies some state outside its scope or has an observable interaction with its calling functions or the outside world.  

看看JS上是如何区分的  

```js
console.log('have side effect');
let outScopeVar = '';

const sideEffectFn = str => outScopeVar = str;

// no side effect but impure function
const location = getCurrentLocation();
```  

## Pure function  
首先这个方法必须满足没有side effect，其次相同的输入必须能得到相同的结果。
```js
let now = Date.now(); // Date.now() is impure function
const getTime = date => date.time(); // pure function
// pure function
const doLoginAction = (username, password) => ({
  type: 'LOGIN_REQUEST',
  payload: {
    username,
    pasword,
  }
});
```  

## Plain object
>
 a set of key-value pairs, created by the {} object literal notation or constructed with new Object()

```js
// return a plain object
const doLoginAction = (username, password) => ({
  type: 'LOGIN_REQUEST',
  payload: {
    username,
    pasword,
  }
});
```

## Currying
>
currying is the technique of translating the evaluation of a function that takes multiple arguments (or a tuple of arguments) into evaluating a sequence of functions, each with a single argument. 

先看看[wiki上的解释](https://en.wikipedia.org/wiki/Currying) 
再来看JS的过程  

```js
// 原始的方法
const origin = (a, b, c) => ({a, b, c});
// 我们只能这样使用
origin(1, 2, 3);
// 根据Currying的定义，我们的函数应该满足以下的特性
origin(1, 2, 3) === origin(1)(2)(3); // should be true
// 除非我们定义成这样
const manualOrigin = a => b => c => ({a, b, c});
// 但是这样的定义的话，原来的方式又不行了
manualOrigin(1, 2, 3); // 这样又不能满足了
// 所以需要点工具函数来帮我们实现这个
// 比如 lodash 函数库中的curry
import _ from 'lodash';
const curryiedOrigin = _.curry(origin);
// 现在我们就可以
curryiedOrigin(1)(2)(3);
// 也可以
curryiedOrigin(1, 2, 3);

```  

至于Haskell中，默认就是currying  

```haskell
origin :: a -> b -> c -> d -- 函数定义
origin a b c = a + b + c

origin 1 2 3 -- 调用函数
```
然后你就会发现 `->` `=>` 不就是原来的数学符号吗？  

## Partial function application
>
Intuitively, partial function application says "if you fix the first arguments of the function, you get a function of the remaining arguments".  

关键字在于`remaining`,剩下的参数,而`currying`则必须是单参数。这就是最大的区别。 
[wiki上的说明](https://en.wikipedia.org/wiki/Currying#Contrast_with_partial_function_application)

## Function composition  
先看看[wiki上的定义](https://en.wikipedia.org/wiki/Function_composition) 

```js
const plus1 = x => x + 1;
const mul3 = x => x * 3;
const minus2 = x => x - 2;
const calc = plus1(mul3(minus2(5)));
// 多参数的函数怎么办？
// currying it
const refact = _.curry((z, y, x) => x * y / z);
const newCalc = refact(4)(2)(plus1(mul3(minus2(5))));
// 这样写太麻烦了，需要个工具函数提供一个方便写法
import compose from "lodash/fp/compose";
const greatCalc = compose(refact(4)(2), plus1, mul3, minus2)(5);
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
