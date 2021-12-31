<template>
  <main>
    <div class="example">
      <Ripple color="#fff" class="button-box" :speed="2">
        <button>Click</button>
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
button {
  width: auto;
  height: 46px;
  line-height: 46px;
  border: none;
  border-radius: 5px;
  padding: 0 1rem;
  color: #fff;
  cursor: pointer;
  background-color: #2ea9df;
}
.example {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 2rem;
}
</style>
