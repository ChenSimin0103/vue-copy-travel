<template>
  <div class="list" ref="wrapper">
    <div>
      <div class="area">
        <div class="title border-topbottom">当前城市</div>
        <div class="button-list">
          <div class="button-wrapper">
            <div class="button">{{this.city}}</div>
          </div>
        </div>
      </div>
      <div class="area">
        <div class="title border-topbottom">热门城市</div>
        <div class="button-list">
          <div class="button-wrapper" v-for="city in hotCities" :key="city.id" @click="handleCityClick(city.name)">
            <div class="button">{{city.name}}</div>
          </div>
        </div>
      </div>
      <div class="area" v-for="(item,index) in cities" :key="index" :ref="index">
        <div class="title border-topbottom">{{index}}</div>
        <div class="item-list">
          <div class="item border-bottom" v-for="city in item" :key="city.id" @click="handleCityClick(city.name)">{{city.name}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Bscroll from "better-scroll";
import { mapState } from "vuex";

export default {
  name: "CityList",
  props: {
    cities: Object,
    hotCities: Array,
    letter: String
  },
  mounted() {
    this.scroll = new Bscroll(this.$refs.wrapper);
  },
  computed: {
    ...mapState(["city"])
  },
  watch: {
    letter() {
      // console.log(this.$refs["A"][0]);
      if (this.letter) {
        const element = this.$refs[this.letter][0];
        // better-scroll的这个方法哼好用
        this.scroll.scrollToElement(element);
      }
    }
  },
  methods: {
    handleCityClick (city) {
      // this.$store.dispatch("changeCity", city);
      this.$store.commit("changeCity", city);
      this.$router.push("/");
    }
  }
};
</script>

<style lang="stylus" scoped>
  @import "~@/assets/styles/varibies.styl";
    .border-topbottom
      &:before
        border-color #ccc
      &:after
        border-color #ccc
    .border-bottom
      &:before
        border-color #ccc
  .list
    overflow hidden
    position absolute
    top 1.58rem
    left 0
    right 0
    bottom 0
    // background red
    .title
      line-height .54rem
      background #eee
      padding-left .2rem
      // color #666
      font-size .26rem
    .button-list
      overflow hidden
      padding .1rem .6rem .1rem .1rem
      .button-wrapper
        float left
        width 33.33%
        .button
          margin .1rem
          padding .1rem 0
          text-align center
          border 1px solid #ccc
          border-radius .03rem
    .item-list
      .item
        height .76rem
        line-height .76rem
        color #666
        padding-left .2rem
</style>
