<template>
  <div ref="list" class="virtual-list-container" @scroll="onScroll($event)">
    <div class="virtual-list-phantom" :style="{ height: listHeight + 'px' }"></div>
    <div class="virtual-list" :style="{ transform: getTransform }">
      <div
        ref="items"
        class="virtual-list-item"
        v-for="item in visibleData"
        :key="item.id"
        :style="{
          height: itemSize + 'px',
          lineHeight: itemSize + 'px',
        }"
      >
        {{ item.value }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // 列表数据
    listData: {
      type: Array,
      default: () => [],
    },
    // 列表元素高度
    itemSize: {
      type: Number,
      default: 200,
    },
  },
  data() {
    return {
      screenHeight: 0, // 可视区域高度
      startOffset: 0, // 偏移量
      startIndex: 0, // 起始索引
      endIndex: null, // 结束索引
    };
  },
  computed: {
    listHeight() {
      return this.listData.length * this.itemSize;
    },
    visibleCount() {
      return Math.ceil(this.screenHeight / this.itemSize);
    },
    visibleData() {
      return this.listData.slice(this.startIndex, Math.min(this.endIndex,this.listData.length));
    },
    getTransform() {
      return `translate3D(0, ${this.startOffset}px, 0)`;
    },
  },
  mounted() {
    this.screenHeight = this.$el.clientHeight;
    this.startIdnex = 0;
    this.endIndex = this.startIndex + this.visibleCount;
  },
  methods: {
    onScroll() {
      //当前滚动位置
      let scrollTop = this.$refs.list.scrollTop;
      //此时的开始索引
      this.startIndex = Math.floor(scrollTop / this.itemSize);
      //此时的结束索引
      this.endIndex = this.startIndex + this.visibleCount;
      //此时的偏移量
      this.startOffset = scrollTop - (scrollTop % this.itemSize);
    },
  },
};
</script>

<style scoped>
.virtual-list-container {
  -webkit-overflow-scrolling: touch;
  position: relative;
  height: 100%;
  overflow: auto;
}
.virtual-list-phantom {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  z-index: -1;
}
.virtual-list {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  text-align: center;
}
.virtual-list-item {
  box-sizing: border-box;
  border-bottom: 1px solid #999;
}
</style>
