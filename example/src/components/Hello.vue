<template>
  <div class="hello">
  <p>
    <pre><code>&lt;vue-countdown-2 deadline=&quot;2018/05/29&quot;&gt;&lt;/vue-countdown-2&gt;</code></pre>
    <strong>剩余：</strong>
    <vue-countdown-2 deadline="2018/05/29"></vue-countdown-2>
  </p><p>
    <pre><code>&lt;vue-countdown-2 deadline=&quot;2018/05/29&quot; format=&quot;%h hours %m minutes %s seconds&quot;&gt;</code></pre>
    <strong>Time Left:</strong>
    <vue-countdown-2 deadline="2018/05/29" format="%h hours %m minutes %s seconds"></vue-countdown-2>
  </p><p>
    <pre><code>&lt;vue-countdown-2 deadline=&quot;2018/05/29&quot; max=&quot;3&quot;&gt;&lt;/vue-countdown-2&gt;</code></pre>
    <strong>剩余：</strong>
    <vue-countdown-2 deadline="2018/05/29" max="3"></vue-countdown-2>
  </p>
  </div>
</template>

<script>
import VueCountdown2 from 'vue-countdown-2';

export default {
  components: {
    VueCountdown2
  },
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

pre {
  background: rgba(0,0,0,.05);
  padding: 20px;
  white-space: pre-wrap;
}
p {
  margin-bottom: 3em;
}
</style>
