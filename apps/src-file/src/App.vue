<template>
  <main>
    <div class="example">
      <Ripple class="love" color="#fff">
        <div class="heart">
          <span class="text">Ejiro</span>
        </div>
      </Ripple>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Ripple from "../../../packages/ui/Ripple.vue";
export default defineComponent({
  name: "App",
  components: {
    Ripple,
  },
});
</script>

<style scoped>
.example {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 2rem;
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
