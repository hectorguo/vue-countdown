# A countdown component for Vue 2.0+

[Demo here](https://hectorguo.com/vue-countdown/)

## Usage

```html
<vue-countdown deadline="2018/05/29"></vue-countdown>

<!-- Output -->
<time>1年 1月 3天 5小时 28分 24秒</time>
```

### Custom Format

```html
<vue-countdown deadline="2018/05/29" format="%h hours %m minutes %s seconds">

<!-- Output -->
<time>82928 hours 27 minutes 46 seconds</time>
```