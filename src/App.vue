<template>
  <div id="app">
    <div class="wrapper">
      <div class="pre" @click.stop="_pre"><</div>
      <transition-group tag="ul" class="list" :name="slideEffect">
        <li class="list-item" v-show="currentIndex === index" v-for="(item, index) in data" :key="index">{{item.content}}</li>
      </transition-group>
      <div class="next" @click.stop="_next">></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  props: {
    data: {
      type: Array,
      default:() =>  [
        {
          url: 'www.zadd7.cn',
          content: '1'
        },
        {
          url: 'www.zadd7.cn',
          content: '2'
        },
        {
          url: 'www.zadd7.cn',
          content: '3'
        },
        {
          url: 'www.zadd7.cn',
          content: '4'
        },
        {
          url: 'www.zadd7.cn',
          content: '5'
        },
        {
          url: 'www.zadd7.cn',
          content: '6'
        }
      ],
    },
    slideEffect: {
      type: String,
      default: 'slide'
    },
    autoSlide: {
      type: Boolean,
      default: true
    },
    slideInterval: {
      type: Number,
      default: 5000
    }
  },
  data () {
    return {
      currentIndex: 0,
      timer: null
    }
  },
  mounted() {
    if (this.autoSlide) {
      this.timer = setInterval(() => {
        this._next();
      }, this.slideInterval);
    }
  },
  methods: {
    setActiveSlide(i) {
      this.currentIndex = i;
    },
    _next() {
      this.$emit('sliderWillSlide', this.data[this.currentIndex], this.data[this.currentIndex+1 > this.data.length-1 ? 0 : this.currentIndex+1]);//派发sliderWillSlide事件
      this.currentIndex < this.data.length-1 ? this.currentIndex++ : this.currentIndex = 0;
      setTimeout(() => {//slider动画完成后派发sliderDidSlide事件
        this.$emit('sliderDidSlide', this.data[this.currentIndex], this.data[this.currentIndex-1 < 0 ? this.data.length-1 : this.currentIndex-1])
      }, 1000);
    },
    _pre() {
      this.$emit('sliderWillSlide', this.data[this.currentIndex], this.data[this.currentIndex+1 > this.data.length-1 ? 0 : this.currentIndex+1]);//派发sliderWillSlide事件
      this.currentIndex !== 0 ? this.currentIndex-- : this.currentIndex = this.data.length-1;
      setTimeout(() => {//slider动画完成后派发sliderDidSlide事件
        this.$emit('sliderDidSlide', this.data[this.currentIndex], this.data[this.currentIndex-1 < 0 ? this.data.length-1 : this.currentIndex-1])
      }, 1000);
    }
  },
  destroyed() {
    this.timer = null;
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  user-select: none;
}
.slide-enter, .slide-leave-to{
  transform: translateX(100%);
}
.fadeout-enter, .fadeout-leave-to{
  opacity: 0;
}
.wrapper{
  position: relative;
  width: 100%;
  height: 200px;
}
.wrapper .pre,
.wrapper .next,
.list,
.list .list-item{
  display: inline-block;
  position: absolute;
}
.wrapper .pre,
.wrapper .next{
  position: absolute;
  top: 50%;
  transform: translateY(-20px);
  z-index: 999;
  width: 40px;
  height: 40px;
  font-size: 20px;
  line-height: 1.8;
  font-weight: bold;
  text-align: center;
  background: azure;
  color: #666;
  cursor: pointer;
}
.wrapper .pre{
  left: 0;
}
.wrapper .next{
  right: 0;
}
.list{
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  list-style: none;
}
.list .list-item{
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 200px;
  min-height: 100px;
  transition: all 1s;
}
.list .list-item:nth-of-type(1){
  background: aqua;
}
.list .list-item:nth-of-type(2){
  background: burlywood;
}
.list .list-item:nth-of-type(3){
  background: azure;
}
.list .list-item:nth-of-type(4){
  background: brown;
}
.list .list-item:nth-of-type(5){
  background: violet;
}
.list .list-item:nth-of-type(6){
  background: blanchedalmond;
}
</style>
