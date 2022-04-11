<template>
  <div class="main">
      
      <div class="classss" v-if="this.$route.params.access">
        <div class="count" style="text-align: center; padding: 20px; color: white">
            <h2 > <span style="font-weight: 200"> Appointment Count Today: </span> {{this.count}} </h2>
            <p>{{this.todayDate}}</p>    
        </div>
        <table>
            <tr>
                <th>Name</th>
                <th>Appointment Place</th>
                <th>Appointment Purpose</th>
            </tr>
            <tr v-for="data in this.allData" :key="data.id">
                <td>{{data.name}}</td>
                <td>{{data.placeAppointment}}</td>
                <td>{{data.purposeAppointment}}</td>
            </tr>
        </table>
        <div class="control-button">
            <h1>Access Controls</h1>
            <div class="success" v-if="accessButton">
                <p>{{this.accessButtonText}}</p>
            </div>
            <button @click="QRandTemp()">QR and Temperature</button>
            <button @click="QROnly()">QR Only</button>
            <button @click="TempOnly()">Temperature Only</button>
            <nuxt-link :to="{ name: 'contact', params: { access2: this.$route.params.access  } }">
                    <button class="contact-btn" :disabled="btnDisabled" :style="{backgroundColor: btnColorDisabled}">
                     View Contact Tracing
                    </button>
            </nuxt-link>
        </div>
      </div>
      <div class="classss" v-else>
          <h1>
          Access Denied
      </h1>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            allData: null,
            errored: false,
            count: 0,
            todayDate: '',
            accessButton: false,
            accessButtonText: ''
        }
    },
    mounted() {
        let today = new Date();
        let yyyy = today.getFullYear();
        let mm = today.getMonth() + 1; // Months start at 0!
        let dd = today.getDate();

        if (dd < 10) dd = '0' + dd;
        if (mm < 10) mm = '0' + mm;

        let dateToday = yyyy + '-' + mm + '-' + dd;
        let dateNgayon = mm + "/" + dd + "/" + yyyy
        this.todayDate = dateNgayon

        axios
            .get('http://dt-iotdoorlock.online/site/jsondata?filter[appointmentStart]='+dateToday)
            //.get('http://dt-iotdoorlock.online')
            .then(response => {
            this.allData = response.data
            this.count = Object.keys(response.data).length
            })
            .catch(error => {
            console.log(error)
            this.errored = true
             })
            .finally(() => this.loading = false)
    },
    methods: {
        QRandTemp: function() {
            axios
                .put('http://dt-iotdoorlock.online/api/access/update',{    
                    "id": 1,
                    "QRandTemp": "True",
                    "QROnly": "False",
                    "TempOnly": "False"
                              })
                .then((response) => {
                    this.accessButton = true;
                    this.accessButtonText = "Access Switched to QR and Temperature";
                    console.log(response);
                    this.$emit('postcreated');
                });
        },
        QROnly: function() {
            axios
                .put('http://dt-iotdoorlock.online/api/access/update',{    
                    "id": 1,
                    "QRandTemp": "False",
                    "QROnly": "True",
                    "TempOnly": "False"
                              })
                .then((response) => {
                    this.accessButton = true;
                    this.accessButtonText = "Access Switched to QR Access Only";
                    console.log(response);
                    this.$emit('postcreated');
                });
        },
        TempOnly: function() {
            axios
                .put('http://dt-iotdoorlock.online/api/access/update',{    
                    "id": 1,
                    "QRandTemp": "False",
                    "QROnly": "False",
                    "TempOnly": "True"
                              })
                .then((response) => {
                    this.accessButton = true;
                    this.accessButtonText = "Access Switched to Temperature Access Only";
                    console.log(response);
                    this.$emit('postcreated');
                });
        },
    }
}
</script>

<style lang="scss">
$primary-color: #3598DC;
    .main {
        font-family: Arial, Helvetica, sans-serif;
        width: 100vw;
        min-height: 100vh;
        background-color: $primary-color;
        .classss {
            h1 {
                padding-top: 20px;
                text-align: center;
                color: white;
            }
            .control-button {
                margin-left: 10%;
                margin-right: 10%;
                width: 80%;
                padding-top: 15px;
                text-align: center;
                padding-bottom: 30px;

                .success {
                    background-color: #8AFF80;
                    height: auto;
                    margin: auto;
                    padding: 1% 0;
                    width: 100%;
                    padding-left: 18px;
                    text-align: center;
                }

                h1 {
                    color: white;

                }
                button {
                    background-color: white;
                    color: $primary-color;
                    height: 38px;
                    width: 100%;
                    margin-top: 10px;
                }

                .contact-btn {
                    background-color: rgb(30, 88, 224);
                    color: white;
                    height: 38px;
                    width: 100%;
                    margin-top: 10px;
                }
            }
            table {
            border: 1px solid #ccc;
            border-collapse: collapse;
            padding: 0;
            margin-left: auto;
            margin-right: auto;
            width: 90%;
            table-layout: fixed;    
            }   

            table caption {
            font-size: 1.5em;
            margin: .5em 0 .75em;
            }

            table tr {
            background-color: #f8f8f8;
            border: 1px solid #ddd;
            padding: .35em;
            }

            table th,
            table td {
            padding: .625em;
            text-align: center;
            }

            table th {
            font-size: .85em;
            letter-spacing: .1em;
            text-transform: uppercase;
            }
            }
        }

        @media screen and (max-width: 600px) {
        .main {
            .classss{
                table {
                    border: 0;
                }

                table caption {
                    font-size: 1.3em;
                }
                
                table thead {
                    border: none;
                    clip: rect(0 0 0 0);
                    height: 1px;
                    margin: -1px;
                    overflow: hidden;
                    padding: 0;
                    position: absolute;
                    width: 1px;
                }
                
                table tr {
                    border-bottom: 3px solid #ddd;
                    display: block;
                    margin-bottom: .625em;
                }
                
                table td {
                    border-bottom: 1px solid #ddd;
                    display: block;
                    font-size: .8em;
                    text-align: right;
                }
                
                table td::before {
                    /*
                    * aria-label has no advantage, it won't be read inside a table
                    content: attr(aria-label);
                    */
                    content: attr(data-label);
                    float: left;
                    font-weight: bold;
                    text-transform: uppercase;
                }
                
                table td:last-child {
                    border-bottom: 0;
                }
            }
        }
        }

</style>