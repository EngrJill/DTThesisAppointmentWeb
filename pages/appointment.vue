<template>
    <div class="main-container">
        <div class="landing-subcontainer">
            <div class="form-container">
                <form action="" class="form">
                    <label for="">First Name</label>
                    <input type="text" placeholder="Input First Name"><br>
                    <label for="">Middle Name</label>
                    <input type="text" placeholder="Input Middle Name"><br>
                    <label for="">Last Name</label>
                    <input type="text" placeholder="Input Last Name"><br>
                    <label for="">Appointment Location</label>
                    <input type="text" placeholder="Input Appointment Location"><br>
                    <div>
                        <date-picker v-model="time1" valueType="format"></date-picker>
                    </div>
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
        time2: null,
        time3: null,
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
            }

        }

    }
</style>