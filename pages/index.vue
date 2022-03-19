<template>
  <div class="main-container">
    <div class="landing-subcontainer">

        <img src="https://images.unsplash.com/photo-1521386455230-4ceaa25b72be?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=387&q=80" alt="lock" class="image">

        <div class="text-container-main">  
          <div class="txt-container">
            <h1>WELCOME TO DT IOT LOCKING SYSTEM WEBSITE</h1>
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
          return '#4B8DFB'
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
    height: 100%;
    width: 100%;
    background-color: $primary-color;
    padding: 100px 0px 100px 0px;
    
    .landing-subcontainer {
      height: 70vh;
      margin-left: 20%;
      margin-right: 20%;
      background-color: white;
      border: solid 1px #bdbdbd;
      box-shadow: #777777 0px 0px 18px;

      .image {
        width: 30%;
        height: 100%;
        object-fit: cover;
      }
      
      .text-container-main {
        width: 70%;
        float: right;

        .txt-container {
          padding-top: 50px;
          margin-inline: auto;
          width: 80%;
          h1 {
            text-align: right;
          }
          h5 {
            font-weight: 300;
            padding-top: 14px;
            font-size: 0.8em;
            text-align: right;
          }
        }

        .placeholder-class {
          padding-top: 30px;
          margin-left: 20px;
          margin-right: 20px;

          p {
            display: flex;
            color: #777777;
            text-align: left;
            padding-left: 6%;
            margin-bottom: 6px;
            span {
              font-size: 0.8em;
              color: orange;
            }
          }

          .checkbox {
            margin-top: 15px;

            p {
              color: black;
              font-size: 14px;
            }

            .cbox {
              cursor: pointer;
              display: flex;
              align-items: center;
              padding: 10px 0;
              padding-left: 32px;

                .checkmark {
                    margin-right: 15px;
                    width:  17px;
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

          .name {
            background-color: #FFFFFF;
            height: 6vh;
            border: #777777 solid 1px;
            width: 90%;
            padding-left: 7px;
            margin-left:6%;
          }
          button {
            background-color: #4B8DFB;
            color: white;
            height: 6vh;
            width: 90%;
            margin-left: 30px;
            margin-right: auto;
          }
          }
      }

    }

   
  }

   @media screen and (max-width:950px) {
      .main-container {
        .landing-subcontainer {
          margin-left: 5%;
          margin-right: 5%;
        }
      }
    }

    @media screen and (max-width:611px) {
      .main-container {
        h1 {
          font-size: 2em;
        }
        .landing-subcontainer {
          height: 85vh;
        }
      }
    }

    @media screen and (max-width:466px) {
      .main-container {
        h1 {
          font-size: 2em;
        }
        .landing-subcontainer {
          height: 95vh;
          .placeholder-class {
            button {
              margin-left: 0;
              margin-right: auto;
            }
          }
        }
      }
    }

</style>