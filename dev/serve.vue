<script lang="ts">
import { defineComponent } from "vue";
import Vue3TouchRipple from "@/vue3-touch-ripple.vue";

export default defineComponent({
  name: "ServeDev",
  components: {
    Vue3TouchRipple,
  },
});
</script>

<template>
  <div id="app">
    <div class="example">
      <vue3-touch-ripple
        class="div-box box-1"
        :speed="2"
        :opacity="0.3"
        color="#999"
        transition="cubic-bezier(0.18, 0.89, 0.32, 1.28)"
      >
        <div class="box">
          <code>cubic-bezier(0.18, 0.89, 0.32, 1.28)</code>
        </div>
      </vue3-touch-ripple>
    </div>

    <div class="example">
      <vue3-touch-ripple class="love" color="#fff">
        <div class="heart">
          <span class="text">Love</span>
        </div>
      </vue3-touch-ripple>
    </div>
  </div>
</template>
<style scoped>
.example {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 2rem;
}
.box-1 {
  background: #36485e;
}
.div-box {
  width: calc(50% - 1rem);
  height: 14rem;
  line-height: 14rem;
  text-align: center;
  color: #fff;
}
.example .love {
  cursor: pointer;
}
.example .love > .heart {
  position: relative;
  width: 420px;
  height: 360px;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
}
.example .love > .heart > .text {
  color: white;
  z-index: 999999;
  font-size: 30px;
  margin-bottom: 60px;
  margin-left: 20px;
  user-select: none;
}
.example .love > .heart:before,
.example .love > .heart:after {
  position: absolute;
  content: "";
  left: 220px;
  top: 10px;
  width: 220px;
  height: 340px;
  background: #fc2e5a;
  border-radius: 200px 200px 0 0;
  transform: rotate(-45deg);
  transform-origin: 0 100%;
}
.example .love > .heart:after {
  left: 0;
  transform: rotate(45deg);
  transform-origin: 100% 100%;
}
</style>
