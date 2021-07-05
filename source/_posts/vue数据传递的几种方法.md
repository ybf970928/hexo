---
title: vue数据传递的几种方法
---

##### 父子传递数据

+ 比如3个嵌套组件,如果还是通过$emit传递数据,需要重复写多个方法,影响效率.
所以嵌套不是太深的话可以使用this.$parent.emit(),还可以通过这个方法获取$ref等等其他属性
+ 如果多个组件嵌套,可以自定义事件,在vue原型上更改

```js
export default function(vue){
//子组件想父组件传递数据
    Vue.prototype.$eventDispath=function(name,value){
        let parent=this.$parent
        while(parent){
            parent.$emit(name,value)
            parnet=parent.$parent
        }
    }
//父组件像子组件传递事件
Vue.prototype.$eventThing=function(name,value){
        const bc=(children)=>{
            children.map(ele=>{
            ele.$emit(name.value)
            if(ele.$children){
                bc(ele.$children)
             }
            )
        }
    }
    bc(this.$children)
}
```
<!--more-->

#### 兄弟组件之间的传值
+ 通过new vue ()挂载到vue的propototype原型上,主要获取$on和$emit两个方法
+ 首先通过$on来注册一个事件,再通过$emit来触发这个事件
+ 注意: 执行的先后顺序问题,使用this.$nexttick()
```js
// 主要使用vue的 $on 和 $emit
Vue.prototype.$eventBus=new Vue()
```

#### 父子之间传递属性
+ 父组件设置一些属性值,需要传递到孙子组件上,没必要再props接受,可以通过this.$attrs获取属性,在孙子组件用v-bind(用于设置属性)直接传递这个属性,然后再孙子组件页面直接通过this.$attrs.XXX获取

#### 父子之间传递方法
+ 在子组件绑定事件,通过this.$listeners获取到这个方法,之后在孙子组件里通过v-on给他绑定这个事件

#### 使用依赖注入
+ 在父组件中使用
``` js
 provide(){
    //他可以返回一个数据
    return {
        grandpa:this  // <--可以是任意数据
    }
}
```
+ 子组件接受通过
``` js
//数据注入,他是一个数组
inject:['grandpa']
```
