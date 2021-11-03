<template>
    <div class="main-container">
        <div class="landing-subcontainer">
            <div class="form-container">
            <h3>Kindly fill-out this form</h3>
                <form action="" class="form">
                    <label for="first_name">First Name</label><br>
                    <input type="text" placeholder="Input First Name" v-model="first_name"><br>
                    <label for="">Last Name</label><br>
                    <input type="text" placeholder="Input Last Name"><br>
                    <label for="">Contact Number</label><br>
                    <input type="number" placeholder="Input Contact Number"><br>
                    <label for="">Appointment Location</label><br>
                    <input type="text" placeholder="Input Appointment Location"><br>
                    <label for="">Appointment date</label><br>
                    <date-picker style="height: 46px" v-model="time1" valueType="format" placeholder="Please pick a date"></date-picker><br>
                    <nuxt-link to="/timeavailability">
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

  export default {
    components: { DatePicker },
    data() {
      return {
        time1: null,
        first_name: null,

      };
    },
  };     
</script>

<style lang="scss" scoped>
$primary-color: #3598DC;
    .main-container {
    font-family: Arial, Helvetica, sans-serif;
    height: 100vh;
    width: 100vw;
    background-color: $primary-color;
    padding-top: 100px;
    
        .landing-subcontainer {
        height: 80%;
        width: 60%;
        background-color: white;
        margin-left: auto;
        margin-right: auto;
        border-radius: 5px;

            .form-container {
                width: 85%;
                margin-left: auto;
                margin-right: auto;
                
                h3 {
                    padding-top: 50px;
                    padding-bottom: 30px;
                }

                    form {
                        margin-left: auto;
                        margin-right: auto;
                        label {
                            color: #777777;
                        }
                        input {
                            background-color: #F4F1F1;
                            height: 46px;
                            width: 100%;
                            margin-top: 6px;
                            margin-bottom: 20px;
                            border-radius: 5px;
                            padding-left: 18px;
                        }
                        button {
                            background-color: $primary-color;
                            color: white;
                            height: 40px;
                            width: 30%;
                            border-radius: 10px;
                            margin-top: 20px;
                        }
                    }
            }

        }

    }
</style>