<template>
  <div v-show="isRotating" :style="positionStyle" class="drag-tips">{{positionStyle.rotate}}°</div>
</template>    
<script>
import { getComponentRotatedStyle } from "@/utils/style";

export default {
  data() {
    return {
      distance: 10 //提示框与组件底部的距离
    };
  },
  computed: {
    //是否在拖拽旋转中
    isRotating() {
      return this.$store.state.isDragRotating;
    },
    //提示框的定位样式
    positionStyle() {
      if (this.$store.state.curComponent) {
        const { top, left, width, height } = getComponentRotatedStyle(
          this.$store.state.curComponent.style
        );
        let rotate = this.$store.state.curComponent.style.rotate;
        if (rotate < 0) {
          rotate = 360 + rotate;
        } else if (rotate > 360) {
          rotate = Math.abs(rotate - 360);
        }
        rotate = rotate.toFixed(0); //角度保留整数
        return {
          top: top + height + this.distance + "px",
          left: left + width / 2 + "px",
          rotate: rotate
        };
      } else {
        return {};
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.drag-tips {
  position: absolute;
  font-size: 14px;
  background: rgba(247, 247, 247, 0.7);
  border: 1px solid #cbcccc;
  border-radius: 4px;
  padding: 3px 7px;
  transform: translateX(-50%);
}
</style>
