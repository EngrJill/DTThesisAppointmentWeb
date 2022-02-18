<template>
    <div class="main-container">
        <div class="landing-subcontainer">
            <div class="form-container">
            <h3>Hello, {{this.$route.params.nname}}. Kindly fill-out this form</h3>
            <p :style="{display: requiredTrigger}">All fields are required</p>
                <form @submit.prevent="onCreatePost" class="form">
                    <label for="first_name">First Name</label><br>
                    <input type="text" placeholder="Input First Name" v-model="first_name"><br>
                    <label for="">Last Name</label><br>
                    <input type="text" placeholder="Input Last Name" v-model="last_name"><br>
                    <label for="address">Address</label><br>
                    <input type="text" placeholder="Input Address" v-model="address"><br>
                    <label for="">Contact Number</label><br>
                    <input type="number" placeholder="Input Contact Number" v-model="contact_number"><br>
                    <label for="">Email</label><br>
                    <input type="email" placeholder="Input Email" v-model="email"><br>
                    <label for="">Appointment Location</label><br>
                    <input type="text" placeholder="Input Appointment Location" v-model="appointment_location"><br>
                    <label for="">Appointment Purpose</label><br>
                    <input type="text" placeholder="Input Appointment Purpose" v-model="appointment_purpose"><br>
                    <label for="">Appointment date</label><br>
                    <date-picker style="height: 46px; width: min(220px, 50%)" v-model="time1" valueType="format" placeholder="Please pick a date"></date-picker><br>
                    <p :style="{display:dateChecker}">The Date must be a Future Date</p>
                        <div class="success" v-if="isSuccess">
                            <h5>Congratulations, Appointment Approved!</h5>
                            <p>Please click the <strong>View QR</strong> to View your QR Code</p>
                        </div>
                    <button :disabled="btnDisabled2" :style="{backgroundColor: btnColorDisabled2}" type="submit">Verify Appointment</button>
                    <nuxt-link :to="{ name: 'final', params: { pangalan: this.first_name, code: hashFunction(), time: convertToReadableDate(), date: this.time1  } }">
                        <button :disabled="btnDisabled" :style="{backgroundColor: btnColorDisabled}">
                        View QR
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
   import axios from 'axios'

  export default {
    components: { DatePicker, VueQr },
    data() {
      return {
        time1: '',
        address: '',
        first_name: '',
        last_name: '',
        contact_number: '',
        email: '',
        appointment_location: '',
        appointment_purpose: '',
        isSuccess: false,
        qrCode: ''
      };
    },
    computed: {
        btnDisabled: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                this.address.length != 0 &&
                this.contact_number.length != 0 &&
                this.email.length != 0 &&
                this.appointment_location.length != 0 &&
                this.appointment_purpose.length != 0 &&
                this.isSuccess === true &&
                (+new Date() < +new Date(this.time1))
            ) {
                return false
            }
            else {
                return true
            }
        },
        btnColorDisabled: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                this.address.length != 0 &&
                this.contact_number.length != 0 &&
                this.email.length != 0 &&
                this.appointment_location.length != 0 &&
                this.appointment_purpose.length != 0 &&
                this.isSuccess === true &&
                (+new Date() < +new Date(this.time1))

            ) {
                return '#3598DC'
            }
            else {
                return 'gray'
            }
        },
        requiredTrigger: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                this.address.length != 0 &&
                this.contact_number.length != 0 &&
                this.email.length != 0 &&
                this.appointment_location.length != 0 &&
                this.isSuccess === true &&
                this.appointment_purpose.length != 0
            ) {
                return 'none'
            }
            else {
                return 'block'
            }
        },
        btnDisabled2: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                this.address.length != 0 &&
                this.contact_number.length != 0 &&
                this.email.length != 0 &&
                this.appointment_location.length != 0 &&
                this.appointment_purpose.length != 0 &&
                this.isSuccess === false &&
                (+new Date() < +new Date(this.time1))
            ) {
                return false
            }
            else {
                return true
            }
        },
        btnColorDisabled2: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                this.address.length != 0 &&
                this.contact_number.length != 0 &&
                this.email.length != 0 &&
                this.appointment_location.length != 0 &&
                this.appointment_purpose.length != 0 &&
                this.isSuccess === false &&
                (+new Date() < +new Date(this.time1))

            ) {
                return '#3598DC'
            }
            else {
                return 'gray'
            }
        },
        dateChecker: function() {
            let now = +new Date();
            let setDate = +new Date(this.time1)

            if (now > setDate) {
                return 'block'
            } else {
                return 'none'
            }
        }
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

            this.qrCode = hashRaw.hashCode().toString()

            return hashRaw.hashCode()
        },

        convertToReadableDate: function() {
        let timeVar = this.time1
        let month = {'01': "January", 
                    "02": "February", 
                    "03": "March",
                    "04": "April",
                    "05": "May",
                    "06": "June",
                    "07": "July",
                    "08": "August",
                    "09": "September",
                    "10": "October",
                    "11": "November",
                    "12": "December"
                    }
                let readableMonth = timeVar.slice(5,7)
                let readableDate = month[readableMonth] + " " + timeVar.slice(8,10) + ", " + timeVar.slice(0,4)

                return readableDate
            },

        onCreatePost() {
            axios
                .post('http://127.0.0.1:8000/api/user_appointment_details',{
                              "name": this.first_name + " " + this.last_name,
                              "address": this.address,
                              "phoneNumber": this.contact_number,
                              "email": this.email,
                              "appointmentStart": this.time1 + " 00:00:00",
                              "appointmentEnd": this.time1 + " 23:59:59",
                              "placeAppointment": this.appointment_location,
                              "purposeAppointment": this.appointment_purpose,
                              "qrCode": this.qrCode
                              })
                .then((response) => {
                    this.isSuccess = true;
                    console.log(response);
                    this.$emit('postcreated');
                });
        },
    }
  };     
</script>

<style lang="scss" scoped>
$primary-color: #3598DC;
     .main-container {
    font-family: Arial, Helvetica, sans-serif;
    height: 150%;
    width: min(100vw, 100%);
    background-color: $primary-color;
    padding: 100px 0px 100px 0px;
    
        .landing-subcontainer {
        height: 90%;
        width: min(800px, 60%);
        background-color: white;
        margin-inline: auto;
        border-radius: 5px;

            .form-container {
                width: min(800px, 85%);
                margin-inline: auto;
                padding: 0px 0px 100px 0px;
                
                h3 {
                    padding-top: 50px;
                    font-size: clamp(0.8rem, 2vw, 1.3rem);
                }

                p {
                    color: orange;
                    font-size: 0.8em;
                }

                    form {
                        padding-top: 20px;
                        width: min(80ch, 100% - 1rem);
                        margin-inline: auto;
                        font-size: clamp(0.8rem, 1.2vw, 1rem);

                        .success {
                            background-color: #b5ffce;
                            height: 46px;
                            width: min(800px, 100%);
                            margin-top: 6px;
                            border-radius: 5px;
                            padding-left: 18px;
                            font-size: 1em;
                            border: solid rgb(80, 235, 80) 2px;
                                h5 {
                                    margin-top: 1.5%;
                                    font-size: 1.1em;
                                }
                                p {
                                    color: rgb(68, 68, 68)
                                }
                        }

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