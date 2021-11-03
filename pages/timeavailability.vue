<template>
  <div class="main-container">
    <div class="landing-subcontainer">
        <h3 class="title">Please Choose your Availability on January 1, 2022</h3> 
            <div class="time-container">
                <nuxt-link to="/final"> 
                    <button v-for="times in this.timeslots" :key="times.time" :disabled="times.status">
                        {{times.time}}
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
            timeslots: [
                {'time':"8:00 - 8:30", "status": true},
                {'time':"8:30 - 9:00", "status": true},
                {'time':"9:00 - 9:30", "status": true},
                {'time':"9:30 - 10:00", "status": false},
                {'time':"10:00 - 10:30", "status": true},
                {'time':"10:30 - 11:00", "status": true},
                {'time':"11:00 - 11:30", "status": true},
                {'time':"11:30 - 12:00", "status": true},
                {'time':"1:00 - 1:30", "status": false},
                {'time':"1:30 - 2:00", "status": true},
                {'time':"2:00 - 2:30", "status": true},
                {'time':"2:30 - 3:00", "status": true},
                {'time':"3:00 - 3:30", "status": true},
                {'time':"3:30 - 4:00", "status": true},
            ]
        }
    }
}
</script>

<style lang="scss">
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

            .title {
                padding-top: 60px;
                padding-bottom: 40px;
                padding-left: 60px;
            }

            .time-container {
                button {
                    margin-left: auto;
                    margin-right: auto;
                    background-color: $primary-color;
                    border-radius: 50px;
                    height: 50px;
                    width: 150px;
                    color: white;
                    margin-left: 150px;
                    margin-bottom: 20px;
                }

                button:disabled,
                button[disabled]{
                background-color: #c4c4c4;
                color: white;
                }

            }
        }

    }
</style>