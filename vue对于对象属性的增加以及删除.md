## 新增对象属性
>Vue.set(target,key,value)   

>举例
```
person:{
    name:"ling",
    job:"engineer"
    }

Vue.set(vm.person,"age","26")
```
## 删除对象属性
>delete
  原生js用法
>举例：
```js
function fun(){

this.name = 'mm';

}

var obj = new fun();

console.log(obj.name);//mm

delete obj.name;

console.log(obj.name); //undefined
```

