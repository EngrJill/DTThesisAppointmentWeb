
<template>
  <div class="main-container">
    <div class="landing-subcontainer">
      <div class="txt-container">
        <h1>{{codes}}</h1>
        <h1>{{this.$route.params.date}}</h1>
        <vue-qr :bgSrc='src' :logoSrc="src2" :text="hey()" :size="200"></vue-qr>
        <h1>Hell0o world</h1>
      </div>
    </div>
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

<style lang="scss" scoped>
$primary-color: #3598DC;
  .main-container {
    font-family: Arial, Helvetica, sans-serif;
    height: 100vh;
    width: 100vw;
    background-color: $primary-color;
    padding: 100px 0px 0px;
    
    .landing-subcontainer {
      height: 80%;
      width: min(800px, 60%);
      background-color: white;
      margin-inline: auto;
      border-radius: 5px;

      .txt-container {
        padding-top: 50px;
        margin-inline: auto;
        width: min(200px, 65%);

      }
    }
  }
</style>