# script

## 模块定义
```
每个模块都需要包含
export default{

}
```

## data

定义界面交互元素的初始化
```
export default {
  data() {
    return {
      cnt: '0',
      msg: 'abc',
    }
  },
}
```

```
模块内部调用数据时：
this.cnt = '1';
```


## methods

定义所有可用函数
```
export default {
  methods: {
    fun1() { 
      console.log('xx')
    },
    fun2() { 
      console.log('yy')
    },
  },
}
```

```
模块内部调用函数时：
this.fun1()


外部模块调用函数时：
OtherObj.methods.init();
```

## 事件

```
export default {
  created(){ 
    console.log('11')
  },
  mounted() {//页面加载完成后执行的方法,每个组件的初始化需要放在模块内部的mounted中
    console.log('22')
  },
}
```


## 模块引入
引入同一模块多次时，引入后命名虽然不同，但模块的data()会产生覆盖
```
//通过import引用其他VUE组件或js文件
import OtherObj from './components/OtherObj'


//export default 中必须包含当前模块引入的组件
export default {
  components: {//前模块中引入的组件
    OtherObj
  },
}
```

```
引入模块必须在template中使用,编译时直接将OtherObj中的<template>内容展开
<template>
   <OtherObj/>
</template>
```


## props
```
//props中包含当前模板导出的属性及类型
//props中的元素名称与data()初始化中的名称不能共存,且不能通过this.props访问

export default {
  props: {
    msg: String  //【msg为界面的绑定元素】
  },
}

被调用时：
<template>
  <OtherObjName msg="aaaa"/>
</template>
```