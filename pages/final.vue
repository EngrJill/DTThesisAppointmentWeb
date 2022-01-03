
<template>
  <div>
    <Loading v-if="!this.load"></Loading>
    <img src="@/assets/qr.png" alt="">
  </div>
</template>

<script>
export default {
  components: {
    Loading
  },
  data() {
    return {
      load: false
    }
  },
  computed: {
    delay() {
      var delayInMilliseconds = 1000; //1 second

    setTimeout(function() {
      load = true
    }, delayInMilliseconds);
    }
    }
}
</script>

<style>

</style>