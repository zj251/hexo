---
title: 深浅拷贝
---

## 深浅拷贝
😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊
### 浅拷贝：
  **Q1:  什么是浅拷贝呀？**
  
  A1:  仅仅复制了引用，彼此之间的操作会互相影响。比如：两个对象A、B, A有数据, B为空，B复制了A，我们修改A，如果B中的数据跟着变化了
  
  **Q2: 浅拷贝都是怎么实现滴？**
  
  A2：有好几种呀，都在👇
 1. Object.assign()
 2. 展开运算符...
 3. Array.prototype.concat()
 4. Array.prototype.slice()
  **Q3:  来一个🌰呀？** 
 
 
  咱们用Array.prototype.slice()来实现一下浅拷贝
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b4949719bbfc424fa9833924d73c7ddd~tplv-k3u1fbpfcp-watermark.image)

### 深拷贝：
  **Q1:  什么是深拷贝呀？**
  
  A1: 利用递归的方式，递归所有层级的属性。比如：两个对象A、B, A有数据, B为空，B复制了A。不管我们怎么修改A和B，它们都不会互相影响的。
  
  **Q2:  那如何实现深拷贝的呢**
  
   A2: 我平常最常用的就是 `JSON.parser(JSON.stringify(obj))`,可是它不支持正则、函数、以及时间对象。
  
  - JSON.stringify()：把一个js对象序列化为一个JSON字符串
  - JSON.parse()：把JSON字符串反序列化为一个js对象
  
      ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/297e293d01144270b7760300a894b358~tplv-k3u1fbpfcp-watermark.image)
    
    **Q3:  那为什么不支持正则、函数、以及时间对象呢**
    
      1. 如果obj里面有时间对象, 换换后，时间将只是字符串的形式，而不是时间对象
      ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8d0fddb710b44b09836bb262c2ce22fd~tplv-k3u1fbpfcp-watermark.image)
      
      2. 如果obj里有RegExp，则序列化的结果将只得到空对象
      ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b92b956f020e4c00a9c3dbd9cee63465~tplv-k3u1fbpfcp-watermark.image)
      
      3. 如果obj里有函数，undefined，则序列化的结果会把函数或 undefined丢失
      ![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/792098d084b841108ef3edc4da56a13a~tplv-k3u1fbpfcp-watermark.image)
      
**Q3:  实现一个简单的深克隆吧？**

```
function deepClone(obj) {

    // 过滤特殊情况
    // 如果是null或者undefined我就不进行拷贝操作
    if(obj === null) return null;
    if(typeof undefined === "undefined") return undefined;

    // 可能是对象或者普通的值  如果是函数的话是不需要深拷贝
    if(typeof obj !== "object") return obj;

    if(obj instanceof RegExp) {
        return new RegExp(obj);
    }
    if(obj instanceof Date) {
        return new Date(obj);
    }
    // 目的:克隆的结果和之前保持相同的所属的类
    // 不直接创建空对象，找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身  
    let newObj = new obj.constructor;
    for(let key in obj) {
        if(obj.hasOwnProperty(key)) {
            // 递归拷贝
            newObj[key] = deepClone(obj[key])
        }
    }
    return newObj;
}
```
😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊😊