# weex-area-pick


weex-area-pick 基于 alibaba 的weex-ui中的wxc-popup，wxc-overlay 组件开发的三级地址选择器， 感谢weex-ui的开发团队辛苦开源weex-ui。


## 安装

``` sh
npm i weex-area-pick
```
## 示例图

![Image text](https://github.com/xufeiayang/weex-area-pick/blob/master/image/example.jpg)
## 使用方法

``` js
<template>
  <div>
    <text @click="openPicker">打开</text>

     <weex-area-pick v-if="showAreaPicker"
                   :show="showAreaPicker"
                   :defaultAddr="defaultAddr"
                   @closeAreaPicker='close'>
     </weex-area-pick>
  </div>
</template>

<script>

  import weex-area-pick from 'weex-area-pick';

  export default {
    components: { weex-area-pick },
    data: () => ({
      defaultAddr: {
                     province: '北京',
                     city: '北京',
                     dist: '东城区'
                  }, // 可选
      showAreaPicker: false,
    }),
    methods: {
      openPicker () {
        this.showAreaPicker = true;
      },
      close (e) {
        console.log(e)
      }
    }
}
</script>

<style scoped>
</style>

## 使用方法

| Value   |DefaultColumn| Desc                              |
| ------- | --- | --------------------------------- |
| single  |`1`| 单选选择器                        |
| area    |`3`| 区域选择器                        |
| section |`2`| 区间选择器                        |
| date    |`3`| 日期选择器                        |
| time    |`2`| 时间选择器                        |
| linkage |`2`| 联动选择器 (自定义列数联动选择器) |
value type 描述
showAreaPicker 布尔值 是否显示
defaultAddr {      province: '北京',      city: '北京',      dist: '东城区' } 可以传入的默认地址
closeAreaPicker 方法 关闭的回调函数

