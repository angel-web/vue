<template>
<div class="am-scroll" :style="{width: width + 'px',height: height + 'px'}" @mousedown = "mousedown($event)" @mouseup="mouseup" @mousemove.stop="mousemove($event)" @mousewheel.stop = "mousescroll($event)"  @resize = "init">
  <div class="am-scrollbar-vertical" :style="{top: scrollbarTop + 'px', opacity: opacityValue, height: barHeight + 'px'}"></div>
  <div class="am-scroll-container" :style="{top: containerTop + 'px'}">
    <slot name='content'></slot>
    <slot></slot>
  </div>
</div>

</template>

<script>
export default {
  name: 'amScroll',
  props: [
    'width',
    'height'
  ],
  events: {
    amInit () {
      this.init()
    }
  },
  data () {
    return {
      containerHeight: 0,
      contentHeight: 0,
      barHeight: 0,
      isMousedown: false,
      containerTop: 0,
      scrollHeight: 0,
      scrollbarHeight: 0,
      mousedownClientY: 0,
      mousedownContainerTop: 0,
      opacityValue: 0,
      timer: '',
      baseTime: 2,
      verticalOffsetHeight: 40
    }
  },
  computed: {
    scrollbarTop () {
      return -this.containerTop / this.scrollHeight * this.scrollbarHeight
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      let _this = this
      // 获取容器高度.am-scroll
      _this.containerHeight = _this.$el.offsetHeight
      // 获取内容区域高度.amscroll-container
      _this.contentHeight = _this.$el.querySelector('.am-scroll-container').offsetHeight
      // 初始化滚动条高度
      _this.barHeight = _this.containerHeight * _this.containerHeight / _this.contentHeight
      // 初始化内容滚动高度
      _this.scrollHeight = _this.contentHeight - _this.containerHeight
      // 初始化滚动条滚动高度
      _this.scrollbarHeight = _this.containerHeight - _this.barHeight
      // 取消
      document.onmouseup = () => {
        _this.isMousedown = false
        // 隐藏滚动条
        clearInterval(_this.timer)
        _this.interval()
      }
    },
    mousedown (event) {
      let _this = this
      _this.isMousedown = true
      _this.mousedownClientY = event.clientY
      _this.mousedownContainerTop = _this.containerTop
      // 显示滚动条
      _this.opacityValue = _this.baseTime
    },
    mouseup () {
      let _this = this
      _this.isMousedown = false
    },
    mousescroll (event) {
      var _this = this
      var value = event.wheelDelta || -event.detail
      var delta = Math.max(-1, Math.min(1, value)) * 10
      // 显示滚动条
      _this.opacityValue = _this.baseTime
      clearInterval(_this.timer)
      _this.interval()
      if (_this.containerTop + delta >= -_this.scrollHeight && _this.containerTop + delta <= 0) {
        _this.containerTop += delta
      } else if (_this.containerTop + delta < -_this.scrollHeight) {
        _this.containerTop = -_this.scrollHeight
      } else if (_this.containerTop + delta > 0) {
        _this.containerTop = 0
      }
    },
    mousemove (event) {
      let _this = this
      let changY = event.clientY - _this.mousedownClientY
      if (_this.isMousedown) {
        if (_this.mousedownContainerTop + changY >= -_this.scrollHeight && _this.mousedownContainerTop + changY <= 0) {
          this.containerTop = _this.mousedownContainerTop + changY
        } else if (_this.mousedownContainerTop + changY < -_this.scrollHeight) {
          _this.containerTop = -_this.scrollHeight
        } else if (_this.mousedownContainerTop + changY > 0) {
          _this.containerTop = 0
        }
      }
    },
    interval () {
      let _this = this
      _this.timer = setInterval(function () {
        console.log(_this.opacityValue)
        if (_this.opacityValue > 0) {
          _this.opacityValue = (parseInt(_this.opacityValue * 100) - 4) / 100
        } else {
          clearInterval(_this.timer)
        }
      }, 50)
    }
  }
}

</script>

<style>
.am-scroll {
  position: relative;
  width: 100%;
  overflow: hidden;
  user-select:none;
}
.am-scrollbar-vertical {
  position: absolute;
  top: 0;
  right: 0;
  width: 4px;
  height: 100px;
  z-index: 1;
  display: block;
  box-sizing: border-box;
  border: 1px solid rgba(255,255,255,.80196);
  border-radius: 2px;
  background: rgba(0,0,0,.39804);
}
.am-scroll-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
}
</style>
