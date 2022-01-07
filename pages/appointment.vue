<template>
    <div class="main-container">
        <div class="landing-subcontainer">
            <div class="form-container">
            <h3>Kindly fill-out this form</h3>
                <form action="" class="form">
                    <label for="first_name">First Name</label><br>
                    <input type="text" placeholder="Input First Name" v-model="first_name"><br>
                    <label for="">Last Name</label><br>
                    <input type="text" placeholder="Input Last Name" v-model="last_name"><br>
                    <label for="">Contact Number</label><br>
                    <input type="number" placeholder="Input Contact Number" v-model="contact_number"><br>
                    <label for="">Email</label><br>
                    <input type="email" placeholder="Input Email" v-model="email"><br>
                    <label for="">Appointment Location</label><br>
                    <input type="text" placeholder="Input Appointment Location" v-model="appointment_location"><br>
                    <label for="">Appointment Purpose</label><br>
                    <input type="text" placeholder="Input Appointment Purpose" v-model="appointment_purpose"><br>
                    <label for="">Appointment date</label><br>
                    <date-picker style="height: 46px" v-model="time1" valueType="format" placeholder="Please pick a date"></date-picker><br>
                    <nuxt-link :to="{ name: 'final', params: { code: hashFunction(), date: this.time1  } }">
                        <button>
                        PROCEED
                        </button>
                    </nuxt-link>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
   import DatePicker from 'vue2-datepicker';
   import 'vue2-datepicker/index.css';
   import VueQr from 'vue-qr';

  export default {
    components: { DatePicker, VueQr },
    data() {
      return {
        time1: '',
        first_name: '',
        last_name: '',
        contact_number: '',
        email: '',
        appointment_location: '',
        appointment_purpose: '',
      };
    },
    methods: {
            hashFunction: function() {
                function reverseString(str) {
                var newString = "";
                for (var i = str.length - 1; i >= 0; i--) {
                    newString += str[i];
                }
                return newString;
            }


            String.prototype.hashCode = function() {
                    var hash = 0;
                    if (this.length == 0) {
                        return hash;
                    }
                    for (var i = 0; i < this.length; i++) {
                        var char = this.charCodeAt(i);
                        hash = ((hash<<5)-hash)+char;
                        hash = hash & hash; // Convert to 32bit integer
                    }
                    return hash;
                }
            
            let hashRaw = reverseString(this.last_name) + this.time1.toString() + reverseString(this.first_name)

            return hashRaw.hashCode()
        }
    }
  };     
</script>

<style lang="scss" scoped>
$primary-color: #3598DC;
     .main-container {
    font-family: Arial, Helvetica, sans-serif;
    height: 150vh;
    width: 100vw;
    background-color: $primary-color;
    padding: 100px 0px 0px;
    
        .landing-subcontainer {
        height: 90%;
        width: min(800px, 60%);
        background-color: white;
        margin-inline: auto;
        border-radius: 5px;

            .form-container {
                width: min(800px, 85%);
                margin-inline: auto;
                
                h3 {
                    padding-top: 50px;
                    padding-bottom: 30px;
                    font-size: clamp(0.8rem, 2vw, 1.3rem);
                }

                    form {
                        width: min(80ch, 100% - 1rem);
                        margin-inline: auto;
                        font-size: clamp(0.8rem, 1.2vw, 1rem);

                        label {
                            color: #777777;
                            font-size: clamp(0.6rem, 1.2vw, 1rem);
                        }
                        input {
                            background-color: #F4F1F1;
                            height: 46px;
                            width: min(800px, 100%);
                            margin-top: 6px;
                            margin-bottom: 20px;
                            border-radius: 5px;
                            padding-left: 18px;
                            font-size: clamp(0.6rem, 1.3vw, 1rem);
                        }
                        button {
                            background-color: $primary-color;
                            color: white;
                            min-height: 40px;
                            width: min(30vw, 30%);
                            border-radius: 10px;
                            margin-top: 20px;
                            font-size: clamp(0.5rem, 1.5vw, 1rem);
                        }
                    }
            }

        }

    }
</style>