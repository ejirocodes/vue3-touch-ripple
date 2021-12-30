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
import { Ripples } from "@/types/interface";
import { defineComponent, ref, computed, onBeforeUnmount } from "vue";
import RippleCore from "./core.vue";

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
  setup(props) {
    const id = ref(0);
    const ripples = ref<Ripples[]>([]);
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

    // mouse up
    function mouseup() {
      mouseuped.value = true;
      if (keepLastRipple.value) {
        clearRipples();
      }
    }

    // mouse down
    function mousedown(event: MouseEvent) {
      // organise basic attributes
      mouseuped.value = false;

      // Calculate the click position and host container size
      //   @ts-ignore
      const { top: innerY, left: innerX } = inner.value.getBoundingClientRect();
      const { clientX: layerX, clientY: layerY } = event;
      const positionX = layerX - innerX;
      const positionY = layerY - innerY;
      const { size, left, top } = getRippleSize(positionX, positionY);

      // Add animation to queue
      ripples.value.push({
        id: (id.value += 1),
        styles: { size, left, top },
      });
    }

    function handleRippleEnd(id: string) {
      const targetIndex = ripples.value.findIndex(
        (ripple) => ripple?.id === id
      );
      if (targetIndex > -1) {
        // If the mouse is not up, and it is the last animation, the right to delete this animation should be given to the mouse up event
        if (mouseuped.value && targetIndex === ripples.value.length - 1) {
          keepLastRipple.value = true;
          // Otherwise, delete it directly
        } else {
          ripples.value.splice(targetIndex, 1);
        }
      }
    }

    // Get container size
    function getRippleSize(positionX: number, positionY: number) {
      const width = inner.value?.clientWidth;
      const height = inner.value?.clientHeight;

      // Get the point that is the furthest away from the clicked position, and calculate the distance between these two points
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

      // The longest side is the radius
      const maxCoordinate = Math.max.apply({}, coordinates);

      const size = maxCoordinate * 2;
      const left = positionX - size / 2;
      const top = positionY - size / 2;

      return { size, left, top };
    }

    // Empty the animation queue
    function clearRipples() {
      ripples.value = [];
    }
    // The real animation queue ends
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
