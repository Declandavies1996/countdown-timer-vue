<template>
  <div>
    <h3>Hello World</h3>
    <CounterTimer />
  </div>
</template>

<script>
import CounterTimer from '@/components/CountdownTimer.vue';
export default{
    components:{
      CounterTimer
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

</style>
