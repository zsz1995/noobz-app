<template>
  <div ref="scroll" id="scroll-page" class="me">
    <slot></slot>
    <div
      v-loading="loading"
      class="me-loading"
      element-loading-text="拼命加载中..."
      element-loading-spinner="el-icon-loading"
    ></div>
    <el-divider v-if="!loading">
      <i :class="noDataIconClass">{{ noDataTip }}</i>
    </el-divider>
  </div>
</template>

<script>
export default {
  props: {
    loading: Boolean,
    noData: Boolean,
    offset: Number,
    noDataTip: String,
    noDataIconClass: String
  },
  data() {
    return {
      scrollAction: {
        x: undefined,
        y: undefined
      }
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll, false);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    handleScroll() {
      let that = this;
      let topHeight =
        document.documentElement.scrollTop || document.body.scrollTop;
      if (
        topHeight + window.innerHeight >=
          that.$refs.scroll.offsetHeight + that.offset &&
        this.isDownDirection()
      ) {
        if (!that.loading) {
          that.$emit("next-page");
        }
      }
    },
    isDownDirection() {
      if (typeof this.scrollAction.y === "undefined") {
        this.scrollAction.y = window.pageYOffset;
        this.scrollAction.x = window.pageXOffset;
      }
      let diffY = this.scrollAction.y - window.pageYOffset;
      this.scrollAction.y = window.pageYOffset;
      this.scrollAction.x = window.pageXOffset;
      return diffY < 0;
    }
  }
};
</script>
<style scoped>
.el-divider__text {
  background-color: #f5f5f5;
}
.me-loading {
  height: 40px;
}
</style>
