<template>
    <div class="main-container">
        <div class="loading-div" :style="{display: loadingComponent}">
            <LoadingUI/>
            <p>Checking for Availability...</p>
        </div>
        <div class="landing-subcontainer">
            <div class="header">
                <div class="content">
                    <h3>Hello, {{this.$route.params.nname}}. Good Day!</h3>
                    <h7>Kindly fill-out this form for your Appointment</h7>
                    <p :style="{display: requiredTrigger}">All fields are required</p>
                </div>
            </div>
            <div class="form-container">
                <form @submit.prevent="onCreatePost" class="form">
                    <label for="first_name">First Name</label><br>
                    <input type="text" placeholder="Input First Name" v-model="first_name"><br>
                    <label for="">Last Name</label><br>
                    <input type="text" placeholder="Input Last Name" v-model="last_name"><br>
                    <label for="address">Address</label><br>

                    <!-- Dropdown Address Code here! -->
                    <div class="custom-select">
                        <!-- Code for Selecting Region -->
                        <select @change="handleProvince">
                            <option disabled selected>Select Region</option>
                            <option v-for="region in regions" :value="region.region_code" :key="region.region_code">{{
                                region.region_name
                                }}
                            </option>
                        </select>

                            <br/><br/>

                        <!-- Code for Selecting Province -->
                        <select @change="handleCity">
                            <option disabled selected>Select Province</option>
                            <option v-for="province in provinces" :value="province.province_code" :key="province.province_code">
                                {{ province.province_name }}
                            </option>
                        </select>

                            <br/><br/>

                        <!-- Code for Selecting City or Town -->
                        <select @change="handleBarangay">
                            <option disabled selected>Select City</option>
                            <option v-for="city in cities" :value="city.city_code" :key="city.city_code">{{ city.city_name }}</option>
                            </select>
                        <br/><br/>

                        <!-- Code for Selecting Barangay -->
                        <select @change="barangaysChange">
                            <option disabled selected>Select Barangay</option>
                            <option v-for="barangay in barangays" :value="barangay.brgy_code" :key="barangay.brgy_code">{{
                                barangay.brgy_name
                                }}
                            </option>
                        </select>
                        <input type="text" placeholder="House Number/Street/Purok Name" v-model="houseNum"><br>
                    </div>

                    <label for="">Contact Number <span :style="{display: contactTrigger}" style="color: orange; font-size: 0.8rem" class="contact-trigger">*Contact Number must be 11 digit</span></label><br>
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
                        <div class="success" v-if="this.isSuccess">
                            <h5>Appointment Verified!</h5>
                            <p>Please click the <strong>View QR</strong> to View your QR Code</p>
                        </div>
                        <div class="failed" v-if="this.isFailed">
                            <h5>Appointment Failed!</h5>
                            <p>{{verifyMessage}}</p>
                        </div>
                    <button :disabled="btnDisabled2" :style="{backgroundColor: btnColorDisabled2}" type="submit">Verify</button>
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
   import {regions, provinces, cities, barangays} from 'select-philippines-address';
   import LoadingUI from '../components/LoadingUI.vue'

  export default {
    components: { DatePicker, VueQr, LoadingUI },
    data() {
      return {
        availableData: [],
        //availableDataCount: 0,
        time1: '',
        first_name: '',
        last_name: '',
        contact_number: '',
        email: '',
        appointment_location: '',
        appointment_purpose: '',
        isSuccess: false,
        isFailed: false,
        qrCode: '',
        regions: [],
        provinces: [],
        cities: [],
        barangays: [],
        region: '',
        province: '',
        city: '',
        barangay: '',
        houseNum: '',
        isLoading: false,
        verifyMessage: ''
      };
    },
    created() {
        regions().then(response => {
        this.regions = response;
    });
    },
    computed: {
        loadingComponent: function() {
            if (this.isLoading) {
                return 'grid'
            } 
            else {
                return 'none'
            }
        },
        //Button will be disabled (Not Clickable) for Verify Button
        btnDisabled: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                (this.houseNum.length !=0 && this.barangay.length != 0 && this.city.length !=0 && this.province.length !=0 && this.region.length != 0) &&
                this.contact_number.length === 11 &&
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
        //Color Gray if the button is disabled for Verify Button
        btnColorDisabled: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                (this.houseNum.length !=0 && this.barangay.length != 0 && this.city.length !=0 && this.province.length !=0 && this.region.length != 0) &&
                this.contact_number.length === 11 &&
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
        //Orange Text of Required is visible
        requiredTrigger: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                (this.houseNum.length !=0 && this.barangay.length != 0 && this.city.length !=0 && this.province.length !=0 && this.region.length != 0) &&
                this.contact_number.length === 11 &&
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
        //Button will be disabled (Not Clickable) for View QR Button
        btnDisabled2: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                (this.houseNum.length !=0 && this.barangay.length != 0 && this.city.length !=0 && this.province.length !=0 && this.region.length != 0) &&
                this.contact_number.length === 11 &&
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
        //Color Gray if the button is disabled for View QR Button
        btnColorDisabled2: function() {
            if (this.time1.length != 0 && 
                this.first_name.length != 0 &&
                this.last_name.length != 0 &&
                (this.houseNum.length !=0 && this.barangay.length != 0 && this.city.length !=0 && this.province.length !=0 && this.region.length != 0) &&
                this.contact_number.length === 11 &&
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
        //Must be a future date text will be visible
        dateChecker: function() {
            if (+new Date() >= +new Date(this.time1)) {
                return 'block'
            } else {
                return 'none'
            }
        },
        //Must be 11 digit for Contact Number text will be visible
        contactTrigger: function() {
            if (this.contact_number.length != 11) {
                return "block"
            } else {
                return "none"
            }
        }
    },
    methods: {
            //This function converts raw data to hashed random int
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

            return hashRaw.hashCode().toString()
        },

        //This function converts ISO-formatted time (string) to Readable Format
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

        //This Function POST an HTTP request to the server
        onCreatePost() {
            this.isLoading = true
            axios
            .get('http://dt-iotdoorlock.online/site/jsondata?filter[appointmentStart]='+this.time1)
            .then(response => {
                let bool = true;
                let bool2 = false;
                this.availableData = response.data
                let availableDataCount = Object.keys(response.data).length
                console.log(response.data)

                //Check for Duplicate Entry in a Day
                for (let i = 0; i<response.data.length; i++) {
                    if ((this.first_name + " " + this.last_name) == response.data[i].name &&
                        this.email == response.data[i].email) {
                        console.log(response.data[i].name)
                        console.log(response.data[i].email)
                        console.log(response.data[i].address)
                        this.verifyMessage = "Duplicate Entry Found. Please Pick a new date"
                        bool = false
                        break
                    }
                }

                //Check if the number of Appointment Exceeded 21
                if (availableDataCount<21) {
                    bool2 = true
                }
                else {
                    this.verifyMessage = "The Date is Full. Please pick a new date"
                }

                console.log(bool.toString())
                console.log(bool2.toString())
                
                //Assess the 2 logics
                if (bool2 && bool) {
                    axios
                        .post('http://dt-iotdoorlock.online/api/user_appointment_details',{
                                    "name": this.first_name + " " + this.last_name,
                                    "address": this.houseNum+ " " + this.barangay + " " + this.city + " " + this.province + " " + this.region,
                                    "phoneNumber": this.contact_number,
                                    "email": this.email,
                                    "appointmentStart": this.time1 + " 00:00:00",
                                    "appointmentEnd": this.time1 + " 23:59:59",
                                    "placeAppointment": this.appointment_location,
                                    "purposeAppointment": this.appointment_purpose,
                                    "qrCode": this.qrCode
                                    })
                        .then((response) => {
                            this.isLoading = false
                            this.isSuccess = true
                            this.isFailed = false
                            console.log(response);
                            this.$emit('postcreated');
                        })
                }
                else {
                    this.isLoading = false
                    this.isFailed = true
                    this.isSuccess = false
                }
                    })
            .catch(error => {
                console.log(error)
             })
            //  .finally(() => 
            //     this.isLoading = false
            //  )

        },

        //This function handle the automatic fill of dropdown for Province and insert the region on data()
        handleProvince(e) {
            this.region = e.target.selectedOptions[0].text;
            provinces(e.target.value).then(response => {
            this.provinces = response;
            });
        },

        //This function handle the automatic fill of dropdown for City and insert the province on data()
        handleCity(e) {
            this.province = e.target.selectedOptions[0].text;
            cities(e.target.value).then(response => {
            this.cities = response;
            });
        },

        //This function handle the automatic fill of dropdown for Barangay and insert the city on data()
        handleBarangay(e) {
            this.city = e.target.selectedOptions[0].text;
            barangays(e.target.value).then(response => {
                this.barangays = response;
            });
        },

        //This function let you choose the barangay and insert the barangay on data()
        barangaysChange(e) {
            this.barangay = e.target.selectedOptions[0].text;
            },

        //This function 
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

        .loading-div {
            position: fixed;
            top: 0;
            z-index: 999;
            width: 100%;
            height: 100%;
            padding-top: 100px;
            place-items: center;
            background-color: rgba(0,0,0,0.5);
            p {
                margin-top: 70px;
                color: white;
            }
        }
    
        .landing-subcontainer {
            height: 90%;
            width: min(800px, 60%);
            background-color: white;
            margin-inline: auto;

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
                    p {
                        color: orange;
                    }
                }
            }

            .form-container {

                width: min(800px, 85%);
                margin-inline: auto;
                padding: 0px 0px 100px 0px;

                .contact-trigger {
                    span {
                        color: orange;
                    }
                }

                .custom-select {
                    margin-bottom: 13px;

                    input {
                            background-color: #F4F1F1;
                            height: 46px;
                            width: min(800px, 100%);
                            margin-top: 20px;
                            margin-bottom: 20px;
                            padding-left: 18px;
                        }
                    select {
                         /* styling */
                        background-color: #F4F1F1;
                        color:  #1e1e1e;
                        border: solid gray 1px;
                        display: inline-block;
                        font: inherit;
                        line-height: 1.5em;
                        padding: 0.5em 3.5em 0.5em 1em;
                        cursor: pointer;

                        /* reset */

                        margin: 0;      
                        -webkit-box-sizing: border-box;
                        -moz-box-sizing: border-box;
                        box-sizing: border-box;
                        -webkit-appearance: none;
                        -moz-appearance: none; 
                        height: 46px;
                        width: min(800px, 100%);
                    }
                }

                    form {
                        padding-top: 20px;
                        width: min(80ch, 100% - 1rem);
                        margin-inline: auto;

                        .success {
                            background-color: #8AFF80;
                            height: auto;
                            margin: auto;
                            padding: 1% 0;
                            width: 100%;
                            padding-left: 18px;
                            text-align: center;
                                h5 {
                                    font-size: 1.3em;
                                }
                                p {
                                    color: rgb(68, 68, 68);
                                    font-size: 1em;
                                }
                        }

                        .failed {
                            background-color: #f0391d;
                            height: auto;
                            margin: auto;
                            padding: 1% 0;
                            width: 100%;
                            padding-left: 18px;
                            text-align: center;
                                h5 {
                                    font-size: 1.3em;
                                }
                                p {
                                    color: rgb(255, 255, 255);
                                    font-size: 1em;
                                }
                        }

                        label {
                            color: #777777;
                        }
                        input {
                            background-color: #F4F1F1;
                            height: 46px;
                            width: min(800px, 100%);
                            margin-top: 6px;
                            margin-bottom: 20px;
                            padding-left: 18px;
                        }
                        button {
                            background-color: $primary-color;
                            color: white;
                            height: 38px;
                            width:48%;
                            margin-top: 20px;
                        }
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