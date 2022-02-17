
<template>
  <div class="main-container">
    <div class="landing-subcontainer">
      <div class="txt-container">
        <h3>Hi, {{this.$route.params.pangalan}}. </h3>
        <h3>This is DreamTeam01 reminding you about your scheduled appointment on {{this.$route.params.time}}. See you then!</h3>
        <vue-qr  colorDark="#3598DC" colorLight="white" margin="20" :text="hey()" size="200" > </vue-qr>
        <button>Save QR</button>
      </div>
      <div class="txt-notes">
        <h4>We are excited to see you when you arrive at our DreamTeam01 facility!</h4>
        <br>
        <h4>The following are some tips to help you know what to expect once you arrive at our office:</h4> 
        <br>    
            <ol>
              <li>Please arrive on time for your appointment.</li>
              <li>The locking system will take your temperature at the front door.</li>
              <li>The system will confirm your QR Code.</li>
              <li>Masks MUST be worn at all times in all of our facility.</li>
            </ol> 
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
      display: table;
      clear: both;

      .txt-container {
        padding-top: 50px;
        margin-left: 8%;
        width: min(320px, 65%);
        font-size: clamp(0.5rem, 1.3vw, 1rem);
        float: left;
        line-height: 1.3;

          vue-qr {
            border: black;
          }

          button {
          cursor: pointer;
          background-color: $primary-color;
          color: white;
          min-height: 6vh;
          width: min(30vw, 30%);
          border-radius: 10px;
          margin-top: 23%;
          float: right;
          font-size: clamp(0.5rem, 1.5vw, 1rem);
          }

      }
      .txt-notes{
        padding-top: 80px;
        margin-left: 8%;
        float: left;
        width: min(260px, 65%);
        font-size: clamp(0.3rem, 1vw, 0.8rem);
        line-height: 1.8;
      }
    }
  }
</style>