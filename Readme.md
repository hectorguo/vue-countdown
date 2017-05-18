# A countdown component for Vue 2.0+

[Demo here](https://hectorguo.com/vue-countdown/)

## Install

```bash
npm install --save vue-countdown-2
```

## Usage

```html
<template>
    <vue-countdown-2 deadline="2018/05/29"></vue-countdown-2>
</template>

<script>
import VueCountdown2 from 'vue-countdown-2';

export default {
  components: {
    VueCountdown2
  }
}
</script>

<!-- Output -->
<time>1年 1月 3天 5小时 28分 24秒</time>
```

### Custom Format

```html
<vue-countdown-2 deadline="2018/05/29" format="%h hours %m minutes %s seconds"></vue-countdown-2>

<!-- Output -->
<time>82928 hours 27 minutes 46 seconds</time>
```