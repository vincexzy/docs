# vue规范
1.vue方法放置顺序
1.1 components

1.2 props

1.3 data

1.4 created

1.5 mounted

1.6 activited

1.7 update

1.8 beforeRouteUpdate

1.9 metods

1.10 filter

1.11 computed

1.12 watch

2.method 自定义方法命名
2.1 动宾短语（good：jumpPage、openCarInfoDialog）（bad：go、nextPage、show、open、login）

2.2 ajax 方法以 get、post 开头，以 data 结尾（good：getListData、postFormData）（bad：takeData、confirmData、getList、postForm）

2.3 事件方法以 on 开头（onTypeChange、onUsernameInput）

2.4 init、refresh 单词除外

2.5 尽量使用常用单词开头（set、get、open、close、jump）

2.6 驼峰命名（good: getListData）（bad: get_list_data、getlistData）

3.生命周期方法注意点
3.1 不在 mounted、created 之类的方法写逻辑，取 ajax 数据，

3.2 在 created 里面监听 Bus 事件

4.基于模块开发
原则：每一个vue组件首先必须专注于解决一个单一的问题，独立的，可复用的，微小的和可测试的。 如果你的组件做了太多的事或是变得臃肿，请将其拆成更小的组件并保持单一的原则。

5.Vue 组件命名
有意义的: 不过于具体，也不过于抽象
简短: 2 到 3 个单词
具有可读性: 以便于沟通交流
```
<!-- 推荐 -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- 避免 -->
<btn-group></btn-group> <!-- 虽然简短但是可读性差. 使用 `button-group` 替代 -->
<ui-slider></ui-slider> <!-- ui 前缀太过于宽泛，在这里意义不明确 -->
<slider></slider> <!-- 与自定义元素规范不兼容 -->
```
6.验证组件的props
提供默认值。
使用 type 属性校验类型。
使用 props 之前先检查该 prop 是否存在。
```
<template>
  <input type="range" v-model="value" :max="max" :min="min">
</template>
<script type="text/javascript">
  export default {
    props: {
      max: {
        type: Number, // 这里添加了数字类型的校验
        default() { return 10; },
      },
      min: {
        type: Number,
        default() { return 0; },
      },
      value: {
        type: Number,
        default() { return 4; },
      },
    },
  };
</script>
```
7.只在需要时创建组件
Vue.js 是一个基于组件的框架。如果你不知道何时创建组件可能会导致以下问题：

如果组件太大, 可能很难重用和维护;
如果组件太小，你的项目就会（因为深层次的嵌套而）被淹没，也更难使组件间通信;
规则
首先，尽可能早地尝试构建出诸如模态框、提示框、工具条、菜单、头部等这些明显的（通用型）组件。总之，你知道的这些组件以后一定会在当前页面或者是全局范围内需要。

第二，在每一个新的开发项目中，对于一整个页面或者其中的一部分，在进行开发前先尝试思考一下。如果你认为它有一部分应该是一个组件，那么就创建它吧。

最后，如果你不确定，那就不要。避免那些“以后可能会有用”的组件污染你的项目。它们可能会永远的只是（静静地）待在那里，这一点也不聪明。注意，一旦你意识到应该这么做，最好是就把它打破，以避免与项目的其他部分构成兼容性和复杂性。

# 小程序

## Page

```
Page({

  /**
   * 页面的初始数据
   */
  data: {

  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {

  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {

  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {

  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {

  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {

  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {

  },

  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {

  },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  }
})
```

## Component

```
Component({
  /**
   * 组件的属性列表
   */
  properties: {

  },

  /**
   * 组件的初始数据
   */
  data: {

  },

  /**
   * 组件的方法列表
   */
  methods: {

  }
})
```

```
var myBehavior = require('my-behavior')
Component({

  /**
   * behaviors 是用于组件间代码共享的特性，类似于一些编程语言中的“mixins”或“traits”。
   */
  behaviors: [myBehavior],

  /**
   * 组件的属性列表
   */
  properties: {
    myProperty: { // 属性名
      type: String,
      value: ''
    },
    myProperty2: String // 简化的定义方式
  },

  /**
   * 组件的初始数据
   */
  data: {}, // 私有数据，可用于模板渲染

  lifetimes: {
    // 生命周期函数，可以为函数，或一个在methods段中定义的方法名
    attached: function () { },
    moved: function () { },
    detached: function () { },
  },

  // 生命周期函数，可以为函数，或一个在methods段中定义的方法名
  attached: function () { }, // 此处attached的声明会被lifetimes字段中的声明覆盖
  ready: function() { },

  pageLifetimes: {
    // 组件所在页面的生命周期函数
    show: function () { },
    hide: function () { },
    resize: function () { },
  },
  
  /**
   * 组件的方法列表
   */
  methods: {
    onMyButtonTap: function(){
      this.setData({
        // 更新属性和数据的方法与更新页面数据的方法类似
      })
    },
    // 内部方法建议以下划线开头
    _myPrivateMethod: function(){
      // 这里将 data.A[0].B 设为 'myPrivateData'
      this.setData({
        'A[0].B': 'myPrivateData'
      })
    },
    _propertyChange: function(newVal, oldVal) {

    }
  }

})
```