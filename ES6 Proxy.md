# Proxy 

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。

<font style="color:red">Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。</font>

栗子：
```
var obj = {
    name:"caicai",
    age:"123"
}
var obj = new Proxy(obj,{
    get(target,prop){
      return "hello " + target[prop] + " 我是通过proxy拦截器处理后得到的"
    },
    set(target,prop,value){
      console.log(value + " 我是通过proxy set的")
      target[prop] = value + "我是通过proxy set的"
    }
}) 

console.log(obj.name) //hello caicai 我是通过proxy拦截器处理后得到的
obj.name = "菜菜"     //菜菜 我是通过proxy set的
console.log(JSON.styingfy(obj)) //{"name":"hello 菜菜我是通过proxy set的 我是通过proxy拦截器处理后得到的","age":"hello 123 我是通过proxy拦截器处理后得到的"}
```

## Proxy 支持的拦截操作

（1）get(target, propKey, receiver)

拦截对象属性的读取，比如proxy.foo和proxy['foo']。

最后一个参数receiver是一个对象，可选，参见下面Reflect.get的部分。

（2）set(target, propKey, value, receiver)

拦截对象属性的设置，比如proxy.foo = v或proxy['foo'] = v，返回一个布尔值。

（3）has(target, propKey)

拦截propKey in proxy的操作，返回一个布尔值。

（4）deleteProperty(target, propKey)

拦截delete proxy[propKey]的操作，返回一个布尔值。

（5）ownKeys(target)

拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。

（6）getOwnPropertyDescriptor(target, propKey)

拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。

（7）defineProperty(target, propKey, propDesc)

拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。

（8）preventExtensions(target)

拦截Object.preventExtensions(proxy)，返回一个布尔值。

（9）getPrototypeOf(target)

拦截Object.getPrototypeOf(proxy)，返回一个对象。

（10）isExtensible(target)

拦截Object.isExtensible(proxy)，返回一个布尔值。

（11）setPrototypeOf(target, proto)

拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。

如果目标对象是函数，那么还有两种额外操作可以拦截。

（12）apply(target, object, args)

拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。

（13）construct(target, args)

拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。
