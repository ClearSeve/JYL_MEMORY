# CLI

## 组件

.vue文件中包含三个标签

```
<template> 对应 html 
<script> 对应 js
<style> 对应 css


<template> 中引用资源时，可直接对应public根目录
<style scoped>定义组件内独立的样式
```


## script
```
//通过import引用其他VUE组件或js文件
//引入的模块中需要包含export default{}导出对应接口
import OtherObjName from './components/OtherObj.vue'



//export default 中包含当前模块引入的组件
export default {
  components: {
    OtherObjName
  },
  props: {
    msg: String
  },
  methods: {//定义页面引用的方法
    fun(){
      console.log("xxx")
    }
  }
}

props中包含当前模板导出的属性及类型，调用时可以
【msg为界面的交互元素】
<template>
  <OtherObjName msg="aaaa"/>
</template>
```
