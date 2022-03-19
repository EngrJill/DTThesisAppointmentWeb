
<template>
  <div class="main-container">
    <div class="landing-subcontainer">
       <div class="header">
          <div class="content">
            <h1> <span style="font-weight: 200" >See you on </span> {{this.$route.params.time}}, {{this.$route.params.pangalan}} </h1>
            <p>These are the Instructions we made for your convenience using our device</p>
          </div>
      </div>
      <div class="txt-container">
          <p>1. Save this QR Code</p>
          <vue-qr  id="vueQr" colorDark="black" colorLight="white" margin=20 :text="this.$route.params.code" style="width: min(500vw, 70%)" size=300 > </vue-qr>
          <a :href="this.vueQrData" :download="this.filename">
            <button @click="downloadQR()">Save QR</button>
          </a>
          <p>2. Tap the Scan QR button on the device monitor </p>
          <p>3. Scan the QR Code</p>
          <p>4. Scan your Temperature by placing your hand in front of the IR Temperature Scanner</p>
          <p>5. Thats it! The Door Lock will unlock by now!</p>
      </div>
    </div>
  </div>
</template>

<script>
import VueQr from 'vue-qr'

export default {
    data() {
      return {
        vueQrData: '',
        filename: this.$route.params.pangalan + "_QRCode.png",
        qrCode: this.$route.params.code
      }
    },
    components: {
      VueQr
    },

    methods: {
      downloadQR: function() {
          let qrLinkData = document.getElementById('vueQr').src 
          return this.vueQrData = qrLinkData
      }
    }
}

</script>

<style lang="scss" scoped>
$primary-color: #3598DC;
  .main-container {
    font-family: Arial, Helvetica, sans-serif;
    height: 100%;
    width: min(100vw, 100%);
    background-color: $primary-color;
    padding: 100px 0px 100px 0px;
    
    .landing-subcontainer {
      height: 70vh;
      width: min(800vw, 60%);
      background-color: white;
      margin-inline: auto;
      display: table;
      clear: both;

      .header {
                height: 120px;
                background-color: aliceblue;
                height: auto;
                margin-left: auto;
                margin-right: auto;
                padding: 10px;
                position: relative;
                
                .content {
                    text-align: center;
                    height: auto;
                    margin: 0 auto;
                    padding: 10px;
                    position: relative;

                    h3 {
                        font-size: 2em;
                        margin-bottom: 10px;
                    }
                }
            }

      .txt-container {
        padding-top: 20px;
        width: min(400px, 65%);
        margin-left: auto;
        margin-right: auto;
        padding-bottom: 40px;

        h3{
        font-size: clamp(0.5rem, 1.3vw, 1.4rem);
        line-height: 1.3;
        }
        p {
          margin-bottom: 7px;
        }

          vue-qr {
            border: black;
          }

          button {
          cursor: pointer;
          background-color: $primary-color;
          color: white;
          min-height: 5vh;
          width: 80%;
          margin-bottom: 15px;
          }

      }
    }
  }

  @media screen and (max-width: 700px) {
    .main-container {
        .landing-subcontainer {
            width: 80%;
            margin-left: 10%;
            margin-right: 10%;
        }
    }
}
</style>