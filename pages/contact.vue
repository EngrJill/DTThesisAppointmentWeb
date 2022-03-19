<template>
  <div class="main-container">
      <input type="text" v-model="name">
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
  </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            name: ''
        }
    },
    mounted() {
         axios
            //.get('http://dt-iotdoorlock.online?filter[appointmentStart]='+dateToday)
            .get('http://dt-iotdoorlock.online')
            .then(response => {
            this.allData = response.data
            this.count = Object.keys(response.data).length
            })
            .catch(error => {
            console.log(error)
            this.errored = true
             })
            .finally(() => this.loading = false)
    }
}
</script>

<style lang="scss">
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

    @media screen and (max-width: 600px) {
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


</style>