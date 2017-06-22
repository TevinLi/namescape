# namescape.js

前些年流传的 JS 命名空间的一种改进版本

- 将命名空间收入闭包保护中，不提供直接访问
- 只允许外界通过指定方法来创建、修改、删除
- 提供命名空间指定命名保护，受保护的命名不允许被覆盖或删除

## 用法

**ns(name, input)**  
注册/读取命名空间

- name：命名空间的命名，必须
- input：命名空间的值，为空不填时表示读取，当不为空时：
	- 非 null 时，表示创建或覆盖
	- 为 null 时，表示删除

**ns.show()**  
显示命名空间所有数据 (仅在console显示，无返回值)

**ns.addProtect(name)**  
添加命名保护

- name：命名空间的命名，必须

**ns.removeProtect(name)**  
移除命名保护
 
- name：命名空间的命名，必须

**ns.showProtect()**  
显示所有受保护的命名 (仅在console显示，无返回值)


## 应用示范

### 在命名空间注册普通对象
```js
//注册
ns('test.something', {
    testA: 1,
    testB: 2
});
//读取
ns('test.something');
//添加保护
ns.addProtect('test.something');
//对保护对象进行操作
ns('test.something', [1, 2]);  //报错，不可以覆盖
ns('test.something', null);    //报错，不可以删除
ns('test', null);              //报错，子级包含受保护的项
//取消保护
ns.removeProtect('test.something');
//删除
ns('test.something', null);  // ->true，删除成功 
```

### 在命名空间注册类
```js
//注册 test.plant
(function () {
    var Plant = ns('test.Plant', function (type) {
        this._type = type;
    });
})();
//注册 test.animal
(function () {
    var Animal = ns('test.Animal', function () {
    });
    Animal.prototype.bark = function (msg) {
        alert(msg)
    };
})();
//读取
(function () {
    //命名空间引用
    var Plant = ns('test.Plant');
    var Animal = ns('test.Animal');
    //业务逻辑
    var tree = new Plant('tree');
    var anm = new Animal();
    anm.bark('ao-!');
})();
```