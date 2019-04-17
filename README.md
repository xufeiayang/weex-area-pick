# weex-area-pick


weex-area-pick 基于 alibaba 的weex-ui中的wxc-popup，wxc-overlay 组件开发的三级地址选择器， 感谢weex-ui的开发团队辛苦开源weex-ui。


## 安装

``` sh
npm i weex-area-pick
```
## 示例图

![Image text](./image/example.png)
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

| Value           |type                                                     
| -------         | ---                                     
| showAreaPicker  |布尔值                                                             |
| defaultAddr     |{province: '北京',city: '北京',dist: '东城区'}                       |
| closeAreaPicker |方法

