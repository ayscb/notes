# 修饰器

修饰器是用来改变类的行为，（注意，只能用在类中，不能用于方法上），其中修饰器是函数。其执行格式为：
``` js

@decorator
Class A{}

// 相当于下面的代码
Class A{}
A = decorator(A) || A;

```
## 1 参数说明
修饰器函数接受3个参数，依次为依次是“目标函数”、“属性名”(可忽略)、“该属性的描述对象”(可忽略)。

``` js
function test(target){
  target.isTestable = true;               //利用修饰器给类添加静态属性
  target.prototype.isTestable = true;     //利用修饰器给类添加动态属性
}

function valid(target, hidden, value) {
  target.hidden = value;  // 是否显示
}

@test
@valid(true)
class A{}

console.log(A.isTestable);       //true
console.log(new A().isTestable);   //true

```

修饰器不仅仅可以修饰类，还可以修饰类的属性和方法：
```
function readonly(target, name, descriptor){
  descriptor.writable = false;
  return descriptor;
}

class Person{
  constructor(name, age, tel){
    this.name = name;
    this.id = id;
  }
  @readonly
  id(){return this.id};
}
```
当然也可以同时调用2个修饰器：
```
function readonly(target, name, descriptor){
  descriptor.writable = false;
  return descriptor;
}
function nonenumerable(target, name, descriptor){
  descriptor.enumerable = false;
  return descriptor;
}

class Person{
  constructor(name, age, tel){
    this.name = name;
    this.id = id;
  }
  @readonly
  @nonenumerable
  id(){return this.id};
}
```
使用修饰器应该注意：虽然类本质是个函数，但修饰器不能用于函数，因为函数具有声明提升。
