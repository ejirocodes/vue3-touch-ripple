
# Vue3-Touch-Ripple

Touch ripple component for Vue 3.x.

## Install

### CDN

``` html
<script type="text/javascript" src="path/to/vue.min.js"></script>
<script type="text/javascript" src="https://unpkg.com/vue3-touch-ripple/"></script>
<script type="text/javascript">
  Vue.use(window.VueTouchRipple, /* { default global options } */)
</script>
```

#### NPM

``` bash
npm install vue3-touch-ripple --save
```

#### mount with component

```javascript
import { touchRipple } from 'vue-touch-ripple'

// import styles
import 'vue-touch-ripple/dist/vue-touch-ripple.css'

export default {
  components: {
    touchRipple
  }
}
```

### Component

```vue
<template>
  <touch-ripple :speed="1" :opacity="0.3" color="#fff" transition="ease">
     <!-- your element... -->
     <h1>it's a h1 title</h1>
     <div>it's a div block</div>
  </touch-ripple>
</template>
```

### Options

| prop       | type     |default |
| :--------  | :----- | :---- |
| speed      | `Number` | `1`    |
| color      | `String` | `#fff` |
| opacity    | `Number` | `0.3`  |
| transition | `String` | `ease-out` |
