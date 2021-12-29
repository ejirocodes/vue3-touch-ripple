<template>
  <div></div>
  <div class="ripple-core" :style="computeCoreStyle"></div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  computed,
  nextTick,
  onMounted,
  onBeforeUnmount,
} from "vue";
import { Timers, CoreStyle } from "../types/interface";

export default defineComponent({
  name: "touch-ripple-core",
  props: {
    id: {
      type: String,
      default: "",
    },
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
    styles: {
      type: Object,
      default: () => ({}),
    },
    transition: {
      type: String,
      default: "all 0.5s ease-in-out",
    },
  },
  emits: ["end"],
  setup(props, { emit }) {
    const timers = ref<Timers>({
      transform: null,
      rippling: null,
    });

    const baseSpeed = ref(0.5);

    const coreStyle = ref<CoreStyle>({
      transform: "scale(0)",
    });

    const computeSpeed = computed(() => {
      return baseSpeed.value / props.speed;
    });

    async function startRipple() {
      await nextTick();
      // start the ripple
      timers.value.transform = setTimeout(() => {
        coreStyle.value.transform = "scale(1)";
      }, 0);

      // End ripples

      timers.value.rippling = setTimeout(() => {
        emit("end", props.id);
      }, computeSpeed.value * 1000);
    }

    const computeCoreStyle = computed(() => {
      return {
        "z-index": props.id,
        opacity: props.opacity,
        top: `${props.styles.top}px`,
        left: `${props.styles.left}px`,
        width: `${props.styles.size}px`,
        height: `${props.styles.size}px`,
        transform: coreStyle.value.transform,
        "background-color": props.color,
        "transition-duration": `${computeSpeed.value}s, 0.4s`,
        "transition-timing-function": `${props.transition}, ease-out`,
      };
    });

    startRipple();

    onBeforeUnmount(() => {
      if (timers.value.transform) {
        clearTimeout(timers.value.transform);
        timers.value.transform = null;
      }
      if (timers.value.rippling) {
        clearTimeout(timers.value.rippling);
        timers.value.rippling = null;
      }
    });

    onMounted(() => {
      startRipple();
    });
    return { computeCoreStyle };
  },
});
</script>

<style scoped></style>
