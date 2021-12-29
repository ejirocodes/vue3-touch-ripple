<template>
  <div
    class="v-touch-ripple"
    @mousedown="mousedown"
    @mouseup="mouseup"
    ref="inner"
  >
    <slot></slot>
    <div class="touch-ripple" v-show="showRipple">
      <transition-group
        tag="div"
        class="ripple-inner"
        name="ripple"
        @enter="rippleEnter"
        @after-leave="rippleLeave"
      >
        <ripple-core
          v-for="ripple in ripples"
          :key="ripple.id"
          :id="ripple.id"
          :color="color || options.color"
          :speed="speed || options.speed"
          :opacity="opacity || options.opacity"
          :transition="transition || options.transition"
          :styles="ripple.styles"
          @end="handleRippleEnd"
        />
      </transition-group>
    </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable @typescript-eslint/ban-ts-comment */
import { defineComponent, ref, computed, onBeforeUnmount } from "vue";
import RippleCore from "./Core.vue";

export default defineComponent({
  name: "touch-ripple",
  props: {
    color: {
      type: String,
      default: "#fff",
    },
    opacity: {
      type: [Number, String],
      default: 0.2,
    },
    speed: {
      type: [Number],
      default: 0.5,
    },
    globalOptions: {
      type: Object,
      default: () => ({}),
    },
    transition: {
      type: String,
      default: "all 0.5s ease-in-out",
    },
  },
  components: {
    RippleCore,
  },
  emits: ["end"],
  setup(props, { emit }) {
    const id = ref(0);
    const ripples = ref([]);
    const rippleCount = ref(0);
    const mouseuped = ref(true);
    const keepLastRipple = ref(false);
    const inner = ref<HTMLDivElement | HTMLElement | null>(null);

    const showRipple = computed(() => {
      return rippleCount.value > 0;
    });
    const options = computed(() => {
      return Object.assign(
        {
          color: "#fff",
          opacity: 0.3,
          speed: 1,
          transition: "ease-out",
        },
        props.globalOptions
      );
    });

    // 鼠标抬起
    function mouseup() {
      mouseuped.value = true;
      if (keepLastRipple.value) {
        clearRipples();
      }
    }

    // 鼠标按下
    function mousedown(event: MouseEvent) {
      // 整理基本属性
      mouseuped.value = false;

      // 计算点击位置、宿主容器尺寸
      //   @ts-ignore
      const { top: innerY, left: innerX } = inner.value.getBoundingClientRect();
      const { clientX: layerX, clientY: layerY } = event;
      const positionX = layerX - innerX;
      const positionY = layerY - innerY;
      const { size, left, top } = getRippleSize(positionX, positionY);

      // 添加动画进队列
      ripples.value.push({
        //   @ts-ignore
        id: (id.value += 1),
        //   @ts-ignore
        styles: { size, left, top },
      });
    }

    function handleRippleEnd(id: string) {
      const targetIndex = ripples.value.findIndex(
        (ripple) => ripple?.id === id
      );
      if (targetIndex > -1) {
        // 如果鼠标未抬起，且是最后一个动画，则这个动画的删除权应该交给鼠标抬起事件
        if (mouseuped.value && targetIndex === ripples.value.length - 1) {
          keepLastRipple.value = true;
          // 否则，直接删除
        } else {
          ripples.value.splice(targetIndex, 1);
        }
      }
    }

    // 获取容器尺寸
    function getRippleSize(positionX: number, positionY: number) {
      const width = inner.value?.clientWidth;
      const height = inner.value?.clientHeight;

      // 得到点击位置距离最远的点，计算出这两点之间的距离
      const square = (x: number) => x * x;
      const coordinates = [
        [0, 0],
        [width, 0],
        [0, height],
        [width, height],
      ].map((coordinate) => {
        return Math.sqrt(
          //   @ts-ignore
          square(coordinate[0] - positionX) + square(coordinate[1] - positionY)
        );
      });

      // 最长边即为半径
      const maxCoordinate = Math.max.apply({}, coordinates);

      const size = maxCoordinate * 2;
      const left = positionX - size / 2;
      const top = positionY - size / 2;

      return { size, left, top };
    }

    // 清空动画队列
    function clearRipples() {
      ripples.value = [];
    }
    // 真实动画队列结束
    function rippleEnter() {
      rippleCount.value += 1;
    }
    function rippleLeave() {
      rippleCount.value -= 1;
    }

    onBeforeUnmount(() => {
      clearRipples();
    });

    return {
      inner,
      mousedown,
      mouseup,
      rippleEnter,
      rippleLeave,
      showRipple,
      options,
      handleRippleEnd,
      ripples,
    };
  },
});
</script>

<style scoped lang="scss">
.v-touch-ripple {
  position: relative;
  display: inline-block;

  > .touch-ripple {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;

    > .ripple-inner {
      position: relative;
      overflow: hidden;
      display: block;
      width: 100%;
      height: 100%;

      > .ripple-core {
        display: block;
        position: absolute;
        border-radius: 50%;
        transition-property: transform, opacity;

        &.ripple-leave-to {
          opacity: 0 !important;
        }
      }
    }
  }
}
</style>
