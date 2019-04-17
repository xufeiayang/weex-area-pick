<template>
  <wxc-popup popup-color="#fff"
             :show="show"
             @wxcPopupOverlayClicked="wxcOverlayBodyClicked"
             pos="bottom"
             :height="700">
    <div class="title">
      <text class="title-text">选择省市区</text>
      <div @click="wxcOverlayBodyClicked" class="button"><text class="but-text">确定</text></div>
    </div>
    <div class="select-item">
      <list class="list-item">
        <cell v-for="(item, index) in areaData" :key="item.title" append="tree" :ref="'province'+index" @click="selectProvince(index)">
          <text class="city-item" :class="[index === selectIndex ? 'active' : 'no-active']">{{item.title}}</text>
        </cell>
      </list>
      <list class="list-item">
        <cell v-for="(item ,index) in areaData[selectIndex].children" :key="item.title" :ref="'city'+index" append="tree" @click="selectCity(index)">
          <text class="city-item" :class="[index === selectCityIndex ? 'active' : 'no-active']">{{item.title}}</text>
        </cell>
      </list>
      <list class="list-item">
        <cell v-for="(item, index) in areaData[selectIndex].children[selectCityIndex].children" :ref="'dist'+index" :key="item.title" append="tree" @click="selectDist(index)">
          <text class="city-item" :class="[index === selectdisIndex ? 'active' : 'no-active']">{{item.title}}</text>
        </cell>
      </list>
    </div>
  </wxc-popup>
</template>

<script>
  import WxcPopup from './wxc-popup';
  import area from './area';
  const dom = weex.requireModule('dom') || {};
  export default {
    components: { WxcPopup },
    name: 'area-pick',
    data () {
      return {
        areaData: area,
        selectIndex: 0,
        selectCityIndex: 0,
        selectdisIndex: 0,
        curArea: {
          province: '北京',
          city: '北京',
          dist: '东城区'
        }
      };
    },
    props: ['show', 'defaultAddr'],
    methods: {
      selectProvince (index) {
        this.selectIndex = index;
      },
      selectCity (index) {
        this.selectCityIndex = index;
      },
      selectDist (index) {
        this.selectdisIndex = index;
      },
      wxcOverlayBodyClicked () {
        this.curArea = {
          province: this.areaData[this.selectIndex].title,
          city: this.areaData[this.selectIndex].children[this.selectCityIndex].title,
          dist: this.areaData[this.selectIndex].children[this.selectCityIndex].children[this.selectdisIndex].title
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
          this.selectdisIndex = this.areaData[this.selectIndex].children[this.selectdisIndex].children.findIndex((value, index, arr) => {
            return value.title === this.curArea.dist;
          });
        }
        setTimeout(() => {
          this.scrollerCell(`province${this.selectIndex}`);
          this.scrollerCell(`city${this.selectCityIndex}`);
          this.scrollerCell(`dist${this.selectdisIndex}`);
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
  .active {
    font-size: 40px;
    color: #ff8400;
  }
  .no-active {
    font-size: 32px;
    color: #666;
  }
  .button {
    background-color: #ff8400;
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
