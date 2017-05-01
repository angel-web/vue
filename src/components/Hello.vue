<template>
<div>
<am-scroll width='400' height='600'>
 <ul>
  <li v-for="index of 50">how are you ?</li>
 </ul>
</am-scroll>
</div>
</template>

<script>
import amScroll from './scroll/scroll'

export default {
  components: {
    amScroll
  }
}

</script>

<style>
* {
  margin: 0;
  padding: 0;
}
.am-scroll {
  border: 1px solid #ddd;
  margin: 20px auto;
}
ul,li{
  list-style: none;
  line-height: 50px;
  text-align: center;
}
li {
  border-bottom: 1px solid #ddd;
}
</style>
