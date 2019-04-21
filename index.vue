<template>
  <wxc-popup popup-color="#fff"
             :show="show"
             @wxcPopupOverlayClicked="wxcOverlayBodyClicked"
             pos="bottom"
             :height="700">
    <div class="title">
      <text class="title-text">选择省市区</text>
      <div @click="wxcOverlayBodyClicked" :style="{'background-color': themeColor || '#ff8400'}" class="button"><text class="but-text">确定</text></div>
    </div>
    <div class="select-item">
      <list class="list-item">
        <cell v-for="(item, index) in areaData" :key="item.title" append="tree" :ref="'province'+index" @click="selectProvince(index)">
          <text class="city-item" :style="index === selectIndex ? textActiveStyle : textStyle">{{item.title}}</text>
        </cell>
      </list>
      <list class="list-item">
        <cell v-for="(item ,index) in areaData[selectIndex].children" :key="item.title" :ref="'city'+index" append="tree" @click="selectCity(index)">
          <text class="city-item" :style="index === selectCityIndex ? textActiveStyle : textStyle">{{item.title}}</text>
        </cell>
      </list>
      <list class="list-item">
        <cell v-for="(item, index) in areaData[selectIndex].children[selectCityIndex].children" :ref="'dist'+index" :key="item.title" append="tree" @click="selectDist(index)">
          <text class="city-item" :style="index === selectDisIndex ? textActiveStyle : textStyle">{{item.title}}</text>
        </cell>
      </list>
    </div>
  </wxc-popup>
</template>

<script>
  import { WxcPopup, WxcOverlay } from 'weex-ui';
  import area from '@/assets/js/area';
  const dom = weex.requireModule('dom') || {};
  export default {
    components: { WxcPopup, WxcOverlay },
    data () {
      return {
        areaData: area, // 省市区数据
        selectIndex: 0, // 省份索引
        selectCityIndex: 0, // 市索引
        selectDisIndex: 0, // 区索引
        curArea: {
          province: '北京',
          city: '北京',
          dist: '东城区'
        },
        textActiveStyle: {
          'color': this.themeColor || '#ff8400',
          'font-size': '40px'
        },
        textStyle: {
          'color': '#666',
          'font-size': '32px'
        },
        clicked: false
      };
    },
    props: ['show', 'defaultAddr', 'themeColor'],
    methods: {
      selectProvince (index) {
        this.selectIndex = index;
      },
      selectCity (index) {
        this.selectCityIndex = index;
      },
      selectDist (index) {
        this.selectDisIndex = index;
      },
      /*
       * @desc 关闭选择器，传回选择的值
       * @param
       * @return
       * @author xufeiyang
       * @time 2019/4/21
      */
      wxcOverlayBodyClicked () {
        this.curArea = {
          province: this.areaData[this.selectIndex].title,
          city: this.areaData[this.selectIndex].children[this.selectCityIndex].title,
          dist: this.areaData[this.selectIndex].children[this.selectCityIndex].children[this.selectDisIndex].title
        };
        this.$emit('closeAreaPicker', this.curArea);
      },
      /*
       * @desc 滚动条滚动到指定位置
       * @param ref 指定的cell
       * @return
       * @author xufeiyang
       * @time 2019/4/15
      */
      scrollerCell (id) {
        const ref = this.$refs[id];
        const el = ref ? ref[0] : null;
        dom.scrollToElement(el);
      }
    },
    created () {
      // 控制滚动条滚动到默认地址
      if (this.defaultAddr) {
        this.curArea = this.defaultAddr;
        if (this.curArea.province) {
          this.selectIndex = this.areaData.findIndex((value, index, arr) => {
            return value.title === this.curArea.province;
          });
        }
        if (this.curArea.city) {
          this.selectCityIndex = this.areaData[this.selectIndex].children.findIndex((value, index, arr) => {
            return value.title === this.curArea.city;
          });
        }
        if (this.curArea.dist) {
          this.selectDisIndex = this.areaData[this.selectIndex].children[this.selectDisIndex].children.findIndex((value, index, arr) => {
            return value.title === this.curArea.dist;
          });
        }
        setTimeout(() => {
          this.scrollerCell(`province${this.selectIndex}`);
          this.scrollerCell(`city${this.selectCityIndex}`);
          this.scrollerCell(`dist${this.selectDisIndex}`);
        }, 800);
      }
    }
  };
</script>

<style>
  .select-item{
    background-color: #fff;
    flex-direction: row;
    flex-wrap: nowrap;
    align-items: center;
    justify-content: center;
    width: 750px;
    padding-top: 20px;
    padding-bottom: 20px;
  }
  .list-item{
    margin-top: 20px;
    margin-bottom: 20px;
    height: 500px;
    flex:1;
  }
  .city-item{
    text-align: center;
    padding-top: 20px;
    padding-bottom: 20px;
  }
  .title {
    flex-direction: row;
    padding-left: 30px;
    padding-right: 30px;
    height: 100px;
    align-items: center;
    border-bottom-color: #e6e6e6;
    border-bottom-width: 2px;
    border-bottom-style: solid;
    justify-content: space-between;
  }
  .title-text {
    font-size: 32px;
    color: #333;
  }
  .button {
    height: 80px;
    width: 200px;
    border-radius: 10px;
    justify-content: center;
    align-items: center;
  }
  .but-text {
    color: #fff;
    font-size: 32px;
  }
</style>
