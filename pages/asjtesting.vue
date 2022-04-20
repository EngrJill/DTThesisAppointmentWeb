<template>
  <div class="main">
      <div class="time-container">
          <h1>{{time}}</h1>
      </div>
      <div class="sub-main">
          <div class="sub-sub-main">
            <label for="capital">Capital in USD</label> <br>
            <input type="number" v-model="capital" placeholder="Input Capital">
            <div class="btn">
                <button @click="btnClick1()" :style="{backgroundColor: btnColorOne}">BUY</button>
                <button @click="btnClick2()" :style="{backgroundColor: btnColorTwo}">SELL</button>
            </div> 
            <table>
                <tr>
                    <td>Lot Size</td>
                    <td class="bold">{{(capital/850).toFixed(2)}}</td>
                </tr>
                <tr>
                    <td>Threshold by 2</td>
                    <td class="bold">{{((capital/850)*100*0.3*2).toFixed(2)}}</td>
                </tr>
                <tr>
                    <td>Threshold by 2.1</td>
                    <td class="bold">{{((capital/850)*100*0.3*2.1).toFixed(2)}}</td>
                </tr>
                <tr>
                    <td>Threshold by 2.2</td>
                    <td class="bold">{{((capital/850)*100*0.3*2.2).toFixed(2)}}</td>
                </tr>
                <tr>
                    <td>Threshold by 2.3</td>
                    <td class="bold">{{((capital/850)*100*0.3*2.3).toFixed(2)}}</td>
                </tr>
                <!-- <tr>
                    <td>PHP Conversion</td>
                    <td class="bold">Php {{(this.capital*conversionRate).toLocaleString()}}</td>
                </tr> -->
            </table>
            
            <!-- <h5>Lot Size:<span>{{(capital/850).toFixed(2)}}</span></h5>
            <h5>Threshold by 2:<span>{{((capital/850)*100*0.3*2).toFixed(2)}}</span></h5>
            <h5>Threshold by 2.2:<span>{{((capital/850)*100*0.3*2.2).toFixed(2)}}</span></h5>
            <h5>Threshold by 2.3:<span>{{((capital/850)*100*0.3*2.3).toFixed(2)}}</span></h5>
            <h5>PhP Conversion:<span>Php {{(this.capital*conversionRate).toLocaleString()}}</span> </h5> -->
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            capital: null,
            lotsize: 0,
            threshold: 0,
            conversionRate: null,
            interval: null,
            time: null,
            btnColorOne: 'gray',
            btnColorTwo: 'gray'
        }
    },
    // mounted() {
    //     axios.get('https://free.currconv.com/api/v7/convert?q=USD_PHP&compact=ultra&apiKey=1fda96f33f62aeaa8c51')
    //     .then(response => {
    //         console.log(response.data.USD_PHP);
    //         this.conversionRate = response.data.USD_PHP
    //         // exchange rata data is stored response.result
    //     }).catch(error => {
    //         console.log(error);
    //     });
    // },
    // methods: {
    //     calculate: function() {
    //         this.lotsize = (this.capital/850).toFixed(2)
    //         this.threshold = ((this.capital/850)*100*0.3*2).toFixed(2)
    //     }
    // },

    beforeDestroy() {
    clearInterval(this.interval)
    },
    created() {
        this.interval = setInterval(() => {
        // Concise way to format time according to system locale.
        // In my case this returns "3:48:00 am"
        let a, hh, mm, ss, nhh, nmm, nss;
        a = new Date();

        hh = a.getHours()
        mm = a.getMinutes()
        ss = a.getSeconds()

        hh < 10 ? nhh = "0"+ hh : nhh = hh
        mm < 10 ? nmm = "0" + mm : nmm = mm
        ss < 10 ? nss = "0" + ss : nss = ss 
 
        this.time = nhh + ':' + nmm + ':' + nss
    }, 1000)
    },
    methods: {
        btnClick1: function() {
            if (this.btnColorOne == "gray") {
                this.btnColorOne = "#FF6666"
                this.btnColorTwo = "gray"
                console.log(this.btnColorOne)
            } else {
                this.btnColorOne = "gray"
                this.btnColorTwo = "#FF6666"
                console.log(this.btnColorOne)
            }
            
        },
        btnClick2: function() {
            if (this.btnColorTwo == "gray") {
                this.btnColorTwo = "#FF6666"
                this.btnColorOne = "gray"
                console.log(this.btnColorTwo)
            } else {
                this.btnColorTwo = "gray"
                this.btnColorOne = "#FF6666"
                console.log(this.btnColorTwo)
            }
            //console.log(this.btnColorTwo)
        }
    },
    
}
</script>

<style lang="scss">
$prim-color: 'FF6666';
    .main {
        font-family: Arial, Helvetica, sans-serif;
        background-color: rgb(248, 73, 102);
        height: 100vh;

        .time-container {
            color: white;
            text-align: center;
            padding-top: 30px;
            font-size: 2rem;
        }

        .sub-main {
            position: absolute;
            background-color: white;
            width: 50%;
            left: 50%;
            top: 15%;
            transform: translate(-50%, 0);

            .btn {
                display: flex;
                button {
                    //background-color: rgb(248, 73, 102);
                    // background-color: gray;
                    margin-right: 5px;
                    height: 30px;
                    width:98px;
                    color: white;
                }
                margin-bottom: 10px;
                
            }

            .sub-sub-main {
                label {
                    font-weight: bold;
                    font-size: 1.3rem;
                }
                input {
                    width: auto;
                    height: 40px;
                    background-color: rgb(233, 233, 233);
                    padding-left: 8px;
                    margin-top: 10px;
                    margin-bottom: 20px;
                }
                margin: 40px;
                margin-left: 60px;
                table {
                    td, th {
                    border: 1px solid #dddddd;
                    text-align: left;
                    padding: 8px;
                    }
                    tr:nth-child(even) {
                    background-color: #dddddd;
                    }
                    .bold {
                        font-weight: bold;
                    }
                }
            }
        }
    }

    @media screen and (max-width: 638px) {
        .main {
            .sub-main {
                width: 80%;
                top: 18%;
                .sub-sub-main {
                    margin-left: 40px;
                }
            }
        }
    }
</style>