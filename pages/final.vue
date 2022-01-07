
<template>
  <div>
    <h1>{{codes}}</h1>
    <h1>{{this.$route.params.date}}</h1>
    <vue-qr :bgSrc='src' :logoSrc="src2" :text="hey()" :size="200"></vue-qr>
    <h1>Hell0o world</h1>
  </div>
</template>

<script>
import VueQr from 'vue-qr'

export default {
    components: {
      VueQr
    },
    data(){
      return {
        codes: this.$route.params.code
      }
    },
    methods: {
      hey: function() {
        return this.$route.params.code.toString()
      }
    }
}
</script>

<style>

</style>