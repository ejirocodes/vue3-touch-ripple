<script lang="ts">
import { defineComponent } from 'vue';
import Ripple from "../../../packages/ui/Ripple.vue"

export default /*#__PURE__*/defineComponent({
  name: 'Vue3TouchRipple', // vue component name
  components: {
    Ripple,
  },
});
</script>

<template>
  <Ripple />
</template>
