<template>
  <div class="main-container">
      <div class="landing-subcontainer" v-if="this.$route.params.access2">
          <h1>Entry Logs</h1>
          <label for="">Search a Name</label>
          <input type="text" v-model="name">
          <button @click="checkContactName">Check Name</button>
          <div class="table-container-nameSearch">
              <table>
                <tr>
                    <th>Name</th>
                    <th>Date of Entry</th>
                    <th>Temperature Data</th>
                    <th>Address</th>
                    <th>Appointment Location</th>
                    <th>Contact Number</th>
                </tr>
                <tr v-for="data in this.newDataName" :key="data.id">
                    <td>{{data.name}}</td>
                    <td>{{(data.appointmentStart).substring(0,10)}}</td>
                    <td>{{data.TempData}} Celcius</td>
                    <td>{{data.address}}</td>
                    <td>{{data.appointment_location}}</td>
                    <td>{{data.phoneNumber}}</td>                    
                </tr>
              </table>
          </div>

          <date-picker style="width=80%; margin-left: 30%" v-model="time1" valueType="format" placeholder="Please pick a date"></date-picker><br>
          <button @click="checkContact()">Check Entries</button>
          <div class="table-container-dateSearch" ref="document">
              <table>
                <tr>
                    <th>Name</th>
                    <th>Date of Entry</th>
                    <th>Temperature Data</th>
                    <th>Address</th>
                    <th>Appointment Location</th>
                    <th>Contact Number</th>
                </tr>
                <tr v-for="data in this.newData" :key="data.id" >
                    <td>{{data.name}}</td>
                    <td>{{(data.appointmentStart).substring(0,10)}}</td>
                    <td>{{data.TempData}} Celcius</td>
                    <td>{{data.address}}</td>
                    <td>{{data.placeAppointment}}</td>
                    <td>{{data.phoneNumber}}</td>                    
                </tr>
              </table>
          </div>
          <button @click="exportToPDF" style="backgroundColor: Blue;color:white">Print/Save as PDF</button>
      </div>
      <div class="landing-subcontainer" v-else>
        <h1>Access Denied</h1>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import html2pdf from 'html2pdf.js';

export default {
    components: { DatePicker },
    data() {
        return {
            errored: false,
            name: '',
            allData: [],
            accessEntries: [],
            QRDupess: {},
            QRdupes: {},
            Tempdupes: {},
            time1: '',
            newData: [],
            newDataName: [],
            yow: [],
            yoww: []
        }
    },
    mounted() {
         axios
            .get('http://dt-iotdoorlock.online/site/jsondata')
            .then(response => {
            this.allData = response.data
            })
            .catch(error => {
            console.log(error)
            this.errored = true
             })

        axios
            .get('http://dt-iotdoorlock.online/api/entries')
            .then(response => {
            this.accessEntries = response.data
            
            response.data.forEach((item, index) => {
            this.QRdupes[item.date] = this.QRdupes[item.date] || [];
            this.Tempdupes[item.date] = this.Tempdupes[item.date] || [];
            if ((item.QRentries == "No Content" || item.TempEntries == "No Content") && index != (response.data.length-1)) {
                this.QRdupes[item.date] += ("No QR Data") + ",";
                this.Tempdupes[item.date] += ("No Temperature Data") + ",";
            }
            else if (response.data[(response.data.length) -1] && index == (response.data.length-1)) {
                this.QRdupes[item.date] += (item.QRentries)
                this.Tempdupes[item.date] += (item.TempEntries)
            }
            else {
                this.QRdupes[item.date] += (item.QRentries + ",")
                this.Tempdupes[item.date] += (item.TempEntries + ",")
            }
            });
            })
            .catch(error => {
            console.log(error)
            this.errored = true
             })
    },
    methods: {
        checkContact: function() {

            let QRcodes = ''
            let temps = ''

            for (let i=0; i<Object.keys(this.QRdupes).length; i++) {
                if (this.time1 === Object.keys(this.QRdupes)[i]) {
                    QRcodes = Object.values(this.QRdupes)[i]
                    temps = Object.values(this.Tempdupes)[i]
                }
            }

            let newdata = []

            let QRentries = QRcodes.split(",")
            let TempEntries = temps.split(",")

            for (let i = 0; i<QRentries.length; i++) {
                for (let j =0; j<this.allData.length; j++) {
                    if (QRentries[i] === this.allData[j].qrCode) {
                        this.allData[j].TempData = TempEntries[i]
                        this.allData[j].Date = this.time1
                        newdata.push(this.allData[j])
                    }
                }
            }

            return this.newData = newdata
        },
        checkContactName: function() {
            let QRcodes = ''
            let temps = ''

            for (let i=0; i<Object.keys(this.QRdupes).length; i++) {
                QRcodes += (Object.values(this.QRdupes)[i])
                temps += (Object.values(this.Tempdupes)[i])
            }

            let QRentries = QRcodes.split(",")
            let TempEntries = temps.split(",")

            let allEntries = []

            for (let i = 0; i<QRentries.length; i++) {
                for (let j = 0; j<this.allData.length; j++) {
                    if (QRentries[i] === this.allData[j].qrCode) {
                        allEntries.push(this.allData[j])
                        this.allData[j].TempData = TempEntries[i]
                    }
                }

            }

            let newdataname = []
            let dateCheck = ''

            for (let z = 0; z<allEntries.length; z++) {
                if ((allEntries[z].name).toLowerCase().includes((this.name).toLowerCase())) {
                    dateCheck = allEntries[z].appointmentStart
                }
                if (allEntries[z].appointmentStart === dateCheck) {
                    newdataname.push(allEntries[z])
                    }
            }

            return this.newDataName = newdataname
        },
        exportToPDF () {
				html2pdf(this.$refs.document, {
					margin: 1,
					filename: 'peopleLog.pdf',
					image: { type: 'jpeg', quality: 0.98 },
					html2canvas: { dpi: 192, letterRendering: true },
					jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
				})
		}
    },
            }
</script>

<style lang="scss">
$primary-color: #3598DC;
    .main-container {
        font-family: Arial, Helvetica, sans-serif;
        width: 100vw;
        min-height: 100vh;
        background-color: $primary-color;

        .landing-subcontainer {

            h1 {
                color: white;
                padding-top: 20px;
                text-align: center;
            }
            width: 80%;
            margin-left: 10%;
            margin-right: 10%;

            .table-container-dateSearch, .table-container-nameSearch {
                margin-bottom: 10px;
                table {
                    border: 1px solid #ccc;
                    border-collapse: collapse;
                    margin: 0;
                    padding: 0;
                    width: 100%;
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

            .table-container-dateSearch {
                padding-bottom: 20px;
            }

            button {
                background-color: white;
                color: $primary-color;
                height: 38px;
                width: 80%;
                margin-bottom: 15px;
                margin-left: 10%;
                margin-right: 10%;
            }
            label {
                color: white;
                margin-top: 30px;
                margin-left: 10%;
                margin-right: 10%;
            }
            input {
                background-color: #F4F1F1;
                height: 38px;
                width: 80%;
                margin-top: 10px;
                margin-bottom: 7px;
                margin-left: 10%;
                margin-right: 10%;
                padding-left: 8px;
            }
        }
    }

    

    @media screen and (max-width: 600px) {
    .main-container {
        .landing-subcontainer {
            h1 {
                font-size: 2em;
            }
            width: 90%;
            margin-left: 5%;
            margin-right: 5%;

            .table-container-nameSearch, .table-container-dateSearch {
                font-size: 0.8em;
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
}


</style>