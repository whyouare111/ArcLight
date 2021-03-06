<template>
  <div class="scroll-box">
    <!-- Left Button -->
    <div
      class="scroll scroll-left"
      :class="!button.scrollLeft && 'scroll-disable'"
      @click="scrollLeft(listId, cardId)"
    >
      <v-icon>mdi-chevron-left</v-icon>
    </div>
    <!-- List -->
    <div :id="listId" class="list">
      <slot />
    </div>
    <!-- Right Button -->
    <div
      class="scroll scroll-right"
      :class="!button.scrollRight && 'scroll-disable'"
      @click="scrollRight(listId, cardId)"
    >
      <v-icon>mdi-chevron-right</v-icon>
    </div>
  </div>
</template>

<script>

export default {
  components: {
  },
  props: {
    listId: {
      type: String,
      required: true
    },
    cardId: {
      type: String,
      required: true
    },
    speed: {
      type: Number,
      default: 15
    },
    listUpdate: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      button: {
        scrollRight: false,
        scrollLeft: false
      }
    }
  },
  created () {
    // 侦测页面变化，变化后更新翻页按钮状态。
    this.$nextTick(() => {
      this.scrollButtonUpdate()
      const list = document.getElementById(this.listId)
      window.addEventListener('resize', this.scrollButtonUpdate)
      list.addEventListener('scroll', this.scrollButtonUpdate)
    })
  },
  watch: {
    listUpdate () {
      this.scrollButtonUpdate()
    }
  },
  // 取消侦测器
  destroyed () {
    const list = document.getElementById(this.listId)
    if (list) list.removeEventListener('scroll', this.scrollButtonUpdate)
    window.removeEventListener('resize', this.scrollButtonUpdate)
  },
  methods: {
    /** 更新 翻页按钮状态 */
    scrollButtonUpdate () {
      const list = document.getElementById(this.listId)
      if (list.scrollWidth - list.clientWidth <= 0) {
        this.button.scrollRight = false
        this.button.scrollLeft = false
      } else if (list.scrollLeft >= list.scrollWidth - list.clientWidth) {
        this.button.scrollRight = false
        this.button.scrollLeft = true
      } else if (list.scrollLeft <= 0) {
        this.button.scrollRight = true
        this.button.scrollLeft = false
      } else {
        this.button.scrollRight = true
        this.button.scrollLeft = true
      }
    },
    /** 向左滚动 */
    scrollLeft (id, cardId) {
      if (this.button.scrollLeft) this.singleGo(id, cardId, false)
    },
    /** 向右滚动 */
    scrollRight (id, cardId) {
      if (this.button.scrollRight) this.singleGo(id, cardId, true)
    },
    /** 滚动 */
    singleGo (id, cardId, direction) {
      // 获取 element
      const list = document.getElementById(id)
      const card = document.getElementById(cardId)
      // 获取宽度
      const marginRight = Number(window.getComputedStyle(card).marginRight.replace('px', '')) || 0
      const marginLeft = Number(window.getComputedStyle(card).marginLeft.replace('px', '')) || 0
      const width = list.clientWidth
      const cardWidth = card.clientWidth + marginRight + marginLeft
      // 开始滚动，距离刚好是目前完整显示的卡片所占的宽度
      const distance = Math.floor(width / cardWidth) * cardWidth
      // true 向右滚动、 false 向左滚动
      const scroll = direction ? list.scrollLeft + distance : list.scrollLeft - distance
      this.smoothScroll(list, scroll)
    },
    /** 平滑滚动 */
    smoothScroll (element, scroll) {
      // 决定运动方向
      const realSpeed = Math.round(element.clientWidth / this.speed)
      const speed = element.scrollLeft < scroll ? realSpeed : -realSpeed
      // 最大限位
      const maxSroll = element.scrollWidth - element.clientWidth
      // 持续滚动，直到抵达目标位置。
      let newScroll = false
      let arrive
      const IntervalId = setInterval(() => {
        // 如果抵达目的地，或者触碰边缘，则停止。
        if (newScroll !== false && arrive) {
          element.scrollLeft = scroll
          clearInterval(IntervalId)
          this.scrollButtonUpdate()
        }
        newScroll = element.scrollLeft + speed
        element.scrollLeft = newScroll
        // 检查是否抵达目的
        if (speed > 0) arrive = element.scrollLeft >= scroll || element.scrollLeft >= maxSroll
        else arrive = element.scrollLeft <= scroll || element.scrollLeft <= 0
      }, 10)
    }
  }
}
</script>

<style lang="less" scoped>
.scroll-box {
  position: relative;

  .list {
    display: flex;
    overflow: hidden;
    overflow-x: scroll;
    scrollbar-width: none;
    -ms-overflow-style: none;

    &::-webkit-scrollbar {
      display: none;
    }
  }
  .scroll {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    position: absolute;
    top: 50%;
    bottom: 50%;
    z-index: 1;
    transition: all 0.1s ease-in;

    &.scroll-disable {
      color: #B2B2B2;
      cursor: no-drop;
      i {
        color: #B2B2B2;
      }
      &:hover {
        transform: scale(1);
        i {
          color: #B2B2B2;
          text-shadow: 1px 4px 10px black;
        }
      }
    }

    &:hover {
      transform: scale(1.5);

      i {
        text-shadow: 3px 6px 15px black;
      }
    }

    &.scroll-left {
      left: 0px;
    }

    &.scroll-right {
      right: 0px;
    }

    i {
      font-size: 45px;
      color: white;
      text-shadow: 1px 4px 10px black;
    }
  }
}
</style>
