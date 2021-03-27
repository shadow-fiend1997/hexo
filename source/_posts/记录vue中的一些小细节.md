---
title: 记录vue中的一些小细节
date: 2020-03-27 19:38:46
tags:
---
这篇文章会比较乱，主要是记录一些在使用vue开发的时候遇到的一些小细节

#### hookEvent 监听组件生命周期
 诉求：   
    平时咱们如果使用一些事件监听或者定时器之类的功能，使用后需要在beforeDestroy（）去销毁这些监听，那一般我们会在mouted（）周期里面定义监听，然后在beforeDestroy里去销毁。 监听和销毁的代码间隔很远不太利于维护。
   
所以 我们可以使用hook来监听生命周期。把定义和销毁写到一起去。
  
 ```
export default {
  mounted() {
    this.chart = echarts.init(this.$el)
    // 请求数据，赋值数据 等等一系列操作...

    // 监听窗口发生变化，resize组件
    window.addEventListener('resize', this.$_handleResizeChart)
    // 通过hook监听组件销毁钩子函数，并取消监听事件
    this.$once('hook:beforeDestroy', () => {
      window.removeEventListener('resize', this.$_handleResizeChart)
    })
  },
  updated() {},
  created() {},
  methods: {
    $_handleResizeChart() {
      // this.chart.resize()
    }
  }
}
```
 或者在我们需要监听子组件生命周期的时候使用hook

```
<template>
  <!--通过@hook:updated监听组件的updated生命钩子函数-->
  <!--组件的所有生命周期钩子都可以通过@hook:钩子函数名 来监听触发-->
  <custom-select @hook:updated="$_handleSelectUpdated" />
</template>
<script>
import CustomSelect from '../components/custom-select'
export default {
  components: {
    CustomSelect
  },
  methods: {
    $_handleSelectUpdated() {
      console.log('custom-select组件的updated钩子函数被触发')
    }
  }
}
</script>
```

#### 要修改props传入的参数时
诉求 ： 如果父组件传给子组件一个值，子组件在使用时需要修改这个值

-  如果只是需要按照一定的格式加工 ，可以直接在computer里面创建另一个值，把props里面的值传进去加工。
```
props: {
 name: {
  type: String
 }

  computed: {
   childrenName() {
      return this.name+"的儿子"
    }
  },
```

- 如果需要灵活改变，可以在data里面定义另一个值，初始值使用props里面的值。

```
props: {
 name: {
  type: String
 }

  data: {
   childrenName：this.name
   
  },
```

#### 需要加载一个只做展示的长列表时

诉求 ：如果我们需要展示一个长列表，vue的双向绑定会给长列表遍历 添加get（），set（）。这样比较耗费性能。

```
created(){
	let items = [{name:1,obj:{id:11}},{name:2,obj:{id:22}},{name:3,obj:{id:33}}];
	this.items = Object.freeze(items); 
	// this.items = items;
}

```
使用**Object.freeze()**这样可以使vue不对这个数据添加响应式监听


