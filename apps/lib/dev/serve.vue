<script lang="ts">
import { defineComponent } from 'vue';
import Vue3TouchRipple from '@/vue3-touch-ripple.vue';

export default defineComponent({
  name: 'ServeDev',
  components: {
    Vue3TouchRipple
  }
});
</script>

<template>
  <div id="app">
    <vue3-touch-ripple />
  </div>
</template>
