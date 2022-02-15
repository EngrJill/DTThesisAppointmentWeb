<template>
  <div class="main-container">
    <div class="landing-subcontainer">

      <div class="txt-container">
        <h1>Welcome to Advanced Appointment Systems</h1>
        <h5>Your contact-proof appointment gateway</h5>
      </div>

      <div class="placeholder-class">
          <p>
            Name <span class="requiredTrigger"  :style="{display: requiredTrigger}"> &nbsp; *This field is required</span>
          </p>

          <input type="text" placeholder="What is your name?" class="name" v-model="nname">

          <div class="checkbox">
            <p>By proceeding with this Appointment System Form, 
            you agree with the terms and condition of the people behind this technology</p>
            <label class="cbox" for="agree" :style="{color: disable_black}">
            <input id="agree" type="checkbox" value="agree" v-model="checked" checked>
            <span class="checkmark"></span>
            <span class="label">I Agree</span>
            </label><br>
          </div>

          <nuxt-link :to="{ name: 'appointment', params: { nname: this.nname } }">
            <button :disabled="stillDisable" :style="{backgroundColor: disable_blue}">
              PROCEED
            </button>
          </nuxt-link>
      </div>

    </div>
  </div>
</template>

<script>
export default {
    data() {
      return {
        nname: '',
        checked: false
      }
    },
    computed: {
      disable_black: function() {
        if (this.checked === false) {
          return 'gray'
        }
        else {
          return  'black'
        }
      },
      disable_blue: function() {
        if (this.nname.length != 0 && this.checked) {
          return '#3598DC'
        }
        else {
          return  'gray'
        }
      },
      stillDisable: function() {
        if (this.nname.length === 0 && this.checked) {
          return true
        }
      },
      requiredTrigger: function() {
        if (this.nname.length === 0) {
          return 'block'
        }
        else {
          return 'none'
        }
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
        width: min(500px, 65%);
        h1 {
          font-size: clamp(0.8rem, 3vw, 4rem);
          text-align: center;
        }
        h5 {
          font-size: clamp(0.7rem, 2vw, 1.3rem);
          font-weight: 300;
          padding-top: 10px;
          text-align: center;
        }
      }

      .placeholder-class {
        padding-top: 50px;
        margin-inline: auto;
        width: 65%;

        p {
          display: flex;
          span {
            font-size: 0.8em;
            color: orange;
          }
        }

        .checkbox {
          margin-top: 15px;
          font-size: clamp(0.5rem, 0.10vw + 0.9vw, 0.96rem);

          p {
            color: black;
          }

          .cbox {
            cursor: pointer;
            display: flex;
            align-items: center;
            margin: 10px 0;
            padding-left: 32px;

              .label {
                font-size: 1em;
              }

              .checkmark {
                  margin-right: 15px;
                  width: 17px;
                  height: 17px;
                  border: 2px solid $primary-color;
                  background: $primary-color url(https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/White_check.svg/1200px-White_check.svg.png) center/1250% no-repeat;
              }

              input:checked + .checkmark {
                  background-size: 60%;
                  transition: background-size 0.25s cubic-bezier(0.7, 0, 0.18, 1.24);
                  }

              input {
                display: none;
              }
          }

          
        }

        p {
          color: #777777;
          text-align: left;
          padding-left: 6%;
          font-size: clamp(0.5rem, 1vw, 0.9rem);
        }
        .name {
          background-color: #F4F1F1;
          border-radius: 10px;
          height: 8vh;
          width:min(500px, 95%);
          margin-top: 10px;
          margin-left: 5%;
          padding-left: 14px;
          font-size: clamp(0.5rem, 1vw, 0.9rem);
        }
        button {
          background-color: $primary-color;
          color: white;
          min-height: 6vh;
          width: min(30vw, 30%);
          border-radius: 10px;
          margin-left: 37%;
          font-size: clamp(0.5rem, 1.5vw, 1rem);
        }
      }

    }

  }

</style>