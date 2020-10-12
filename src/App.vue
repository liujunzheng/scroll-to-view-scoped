<template>
<div>
  <p class='arch'>
    当前高亮位置：{{ curPosition }}
    <span @click='onGoNav("one")' :class='{active: curPosition == 1}'>1</span>
    <span @click='onGoNav("two")' :class='{active: curPosition == 2}'>2</span>
    <span @click='onGoNav("three")' :class='{active: curPosition == 3}'>3</span>
    <span @click='onGoNav("four")' :class='{active: curPosition == 4}'>4</span>
  </p>
  <p>当前元素距离消失的像素大小阈值：<input @input='updateEleHeight' v-model='flagSize' type="number"></p>
  <p>意思是：当前块往上滚动时，当前元素的  下边框 距离父元素 上 边缘的距离</p>
  <div id="app">
    <div class="one">
      <h2>One</h2>
      <p :key='n' v-for='n in 10'>1 -- {{  n }}</p>
    </div>
    <div class="two">
      <h2>Two</h2>
      <p :key='n' v-for='n in twoLength'>2 -- {{  n }}</p>
      <button @click='addTwoItem'>添加动态增加高度</button>
    </div>
    <div class="three">
      <h2>Three</h2>
      <p :key='n' v-for='n in threeLength'>3 -- {{  n }}</p>
      <button @click='addThreeItem'>添加动态增加高度</button>
    </div>
    <div class="four">
      <h2>Four</h2>
      <p :key='n' v-for='n in 25'>4 -- {{  n }}</p>
    </div>
  </div>
</div>
</template>

<script>
// eslint-disable-next-line no-console

export default {
  name: 'app',
  components: {},
  data () {
    return {
      flagSize: 50,
      twoLength: 5,
      threeLength: 8,
      eleHeightList: {
        one: 0,
        two: 0,
        three: 0,
        four: 0
      },
      scrollTop: 0,
      isMounted: false
    }
  },
  computed: {
    curPosition () {
      if (!this.isMounted) {
        return ''
      }
      const flagSize = this.flagSize > 0 ? window.parseInt(this.flagSize) : 0
      if (this.scrollTop + flagSize < this.eleHeightList.one) {
        return 1
      } else {
        if (this.scrollTop + flagSize < (this.eleHeightList.one + this.eleHeightList.two)) {
          return 2
        } else {
          if (this.scrollTop + flagSize < (this.eleHeightList.one + this.eleHeightList.two + this.eleHeightList.three)) {
            return 3
          } else {
            return 4
          }
        }
      }
    }
  },
  mounted () {
    this.onAppScroll()
    this.updateEleHeight()
  },
  methods: {
    onAppScroll () {
      window.$('#app').scroll(() => {
        const $scrollTop = window.$('#app').scrollTop()
        console.log('滚动条高度', $scrollTop)
        this.scrollTop = $scrollTop
      })
    },
    addTwoItem () {
      this.twoLength += 5
      this.updateEleHeight()
    },
    addThreeItem () {
      this.threeLength += 5
      this.updateEleHeight()
    },
    updateEleHeight () {
      this.isMounted = false
      setTimeout(() => {
        // console.log(window.$('.four'))
        this.eleHeightList.one = window.$('.one').height()
        this.eleHeightList.two = window.$('.two').height()
        this.eleHeightList.three = window.$('.three').height()
        this.eleHeightList.four = window.$('.four').height()
        this.isMounted = true
      }, 200)
    },
    onGoNav (className) {
      window.$(`.${className}`)[0].scrollIntoView({
        behavior: 'smooth'
      })
    }
  }
}
</script>

<style lang='less'>
*{
  margin: 0;
  padding: 0;
}
#app {
  margin-top: 100px;;
  width: 600px;
  height: 600px;
  border: 1px solid red;
  overflow: auto;
  position: relative;
}
.one{
  background: red;
}
.two{
  background: red;
}
.three{
  background: green;
}
.four{
  background: yellow;
}
.arch{
  width:0% 100%;
  float: right;
  color: red;
  font-size: 26px;
  font-weight: bold;
  text-align: right;
  span{
    display: inline-block;
    width: 50px;
    height: 50px;
    border: 1px solid #000;
    text-align: center;
    &.active{
      border: 3px solid pink;
    }
  }
}
</style>
