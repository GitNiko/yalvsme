Proxy Vs Inheritance Inversion
=================================
两种模式的对比  

## Proxy  
- ref无法直接获取到，需要额外传给wrapper然后再传给组件  
- 会再包一层wrapper组件

## Inheritance Inversion
- 无法作用到函数式组件   
- ref可以直接获取到，如果能确保render()返回的一定是个非函数式组件，则可以使用。否则可能ref回调函数的参数是undefined
- 层次简单,不会额外再包一层wrapper
