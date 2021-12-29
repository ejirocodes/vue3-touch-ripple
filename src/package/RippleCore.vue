<template>
  <div></div>
  <!-- <div class="ripple-core" :style="computeCoreStyle"></div> -->
</template>

<script lang="ts">
import { defineComponent, ref, computed, nextTick } from "vue";
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
      type: [Number, String],
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
  setup(props) {
    const timers = ref<Timers>({
      transform: null,
      rippling: null,
    });

    const baseSpeed = ref(0.5);

    const coreStyle = ref<CoreStyle>({
      transform: "scale(0)",
    });

    async function startRipple() {
      await nextTick();
      // start the ripple
      timers.value.transform = setTimeout(() => {
        coreStyle.value.transform = "scale(1)";
      }, 0);

      // End ripples

      // timers.value.rippling = setTimeout(() => {
      //   this.$emit("end", props.id);
      // }, this.computeSpeed * 1000);

      //  this.$nextTick(() => {

      //       // 开始涟漪
      //       this.timers.transform = setTimeout(() => {
      //         this.coreStyle.transform = 'scale(1)'
      //       }, 0)

      //       // 结束涟漪
      //       this.timers.rippling = setTimeout(() => {
      //         this.$emit('end', this.id)
      //       }, this.computeSpeed * 1000)
      //     })
    }
    return {};
  },
});
</script>

<style scoped></style>
