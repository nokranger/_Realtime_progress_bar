<template>
<div>
    <meta charset="utf-8">
    <meta content="width=device-width,initial-scale=1,minimal-ui" name="viewport">
    <div style="text-align: left;margin-left: 10px;margin-top: 20px;">
      <div style="text-align:center">
        <b-row class="text-center">
          <b-col cols="1"></b-col>
          <b-col style="border: solid 2px #E0e0e0;background: #33c7f7;margin:5px;color:white;" cols="5"><br><label>VIN : {{ VIN }}</label><br></b-col>
          <b-col style="border: solid 2px #E0e0e0;background-color: #33c7f7;margin:5px;color:white;" cols="5"><br><label>OPTION GROUP : {{OPTION_GROUP}}</label><br></b-col>
          <b-col cols="1"></b-col>
          <br>
        </b-row>
      </div>
    </div>
    <div style="text-align: left;margin-left: 10px;margin-top: 20px;">
        <div>
          <!-- <label style="border: solid 2px #E0e0e0;background-color: white;margin:5px;"><label style="margin:5px;">PLAN : <label id="s_plan">{{S_plan}}</label></label></label> -->
          <div>
            <b-row class="text-center">
              <b-col><label style="border: solid 2px #E0e0e0;background-color: white;margin:5px;"><label style="margin:5px;">PLAN : <label id="s_plan">{{S_plan}}</label></label></label></b-col>
              <b-col cols="8">
                  <h1 id="P_time" v-for="(plan_times, index) in plan_time" :key="index" v-rainbow="index"><h1 style="margin:5px;">P{{index}} : {{plan_times}} </h1></h1>
              </b-col>
              <b-col></b-col>
            </b-row>
          </div>
        </div>
        <div>
          <div>
            <b-row class="text-center">
              <b-col></b-col>
              <b-col cols="8">
                <br>
                <b-progress class="mt-2" :max="100" height="4rem" show-value  show-progress animated>
                <b-progress-bar v-for="(plan_times, index) in plan_time" :key="index" :value="plan_times" :variant="index % 5 === 0 ? 'warning' : index % 4 === 0 ? 'success' : index % 3 === 0 ? 'danger' : index % 2 === 0 ? 'primary' : index % 1 === 0 ? 'secondary' : 'dark'"><strong>{{plan_times}}</strong></b-progress-bar>
                </b-progress><br>
              </b-col>
              <b-col>
                <!-- <div id="traffic-light">
                  <input type="radio" name="traffic-light-color" id="color1" value="color1" />
                  <input type="radio" name="traffic-light-color" id="color2" value="color2"/>
                  <input type="radio" name="traffic-light-color" id="color3" value="color3" />
                </div> -->
              </b-col>
            </b-row>
          </div>
        <!-- <br><label style="border: solid 2px #E0e0e0;background-color: white;margin:5px;"><label style="margin:5px;"> ACTUAL : <label id="s_actual">{{S_actual}}</label></label></label> -->
          <div>
            <b-row class="text-center">
              <b-col><br><label style="border: solid 2px #E0e0e0;background-color: white;margin:5px;"><label style="margin:5px;"> ACTUAL : <label id="s_actual">{{S_actual}}</label></label></label></b-col>
              <b-col cols="8">
                <br>
                <b-progress class="mt-2" :max="100" height="4rem" show-value  show-progress animated>
                <b-progress-bar v-for="(actual_times, index) in actual_time" :key="index" :value="actual_times" :variant="index % 5 === 0 ? 'warning' : index % 4 === 0 ? 'success' : index % 3 === 0 ? 'danger' : index % 2 === 0 ? 'primary' : index % 1 === 0 ? 'secondary' : 'dark'"><strong>{{actual_times}}</strong></b-progress-bar>
                </b-progress><br>
              </b-col>
              <b-col>
                <div id="traffic-light">
                  <input type="radio" name="traffic-light-color" id="color1" value="color1" />
                  <input type="radio" name="traffic-light-color" id="color2" value="color2"/>
                  <input type="radio" name="traffic-light-color" id="color3" value="color3" />
                </div>
              </b-col>
            </b-row>
          </div>
          <div style="display: flex;height: 100%;justify-content: center;align-items: center;">
            <b-row class="text-center">
              <b-col style="border: solid 2px #E0e0e0;background-color: #eecb27;margin:5px;color:white;" cols="3.5"><br><label style="margin:5px;">Start : {{start_time}}</label><br><br></b-col>
              <b-col style="border: solid 2px #E0e0e0;background-color: #e13239;margin:5px;color:white;" cols="3.5"><br><label style="margin:5px;">Finish : {{finish_time}}</label><br><br></b-col>
              <b-col style="border: solid 2px #E0e0e0;background-color: #1f1762;margin:5px;color:white;" cols="3.5"><br><label style="margin:5px;">Diff : {{S_plan - S_actual}}</label><br><br></b-col>
              <b-col class="col-auto">
                <b-button style="margin-right:5px;" v-on:click="STOPSV ()" variant="danger">STOP</b-button>
                <b-button v-on:click="HELPSV ()" variant="success">HELP</b-button>
              </b-col>
              <br>
            </b-row>
          </div>
        <div>
        </div>
            <!-- <button v-on:click="STOPSV ()">STOP</button>
            <button v-on:click="HELPSV ()">HELP</button> -->
          <div>
          </div>
        </div>
    </div>
</div>
</template>
<script>
export default {
  data () {
    return {
      VIN: '',
      URN: '',
      OPTION_GROUP: '',
      value: [],
      plan_time: [],
      actual_time: [],
      max_plan_time: 0,
      max_actual_time: 0,
      start_time: '',
      finish_time: '',
      diff: '',
      status: '',
      max: 0,
      Pcolor: '',
      Pcolors: ['Primary', 'Secondary', 'Success', 'Warning', 'danger', 'Info', 'Dark'],
      bars: [
        { variant: 'success', value: 0 },
        { variant: 'info', value: 0 },
        { variant: 'warning', value: 0 },
        { variant: 'danger', value: 0 },
        { variant: 'primary', value: 0 },
        { variant: 'secondary', value: 0 },
        { variant: 'dark', value: 0 }
      ],
      S_plan: 0,
      S_actual: 0
    }
  },
  created () {
    let vm = this
    vm.S_plan = 0
    vm.S_actual = 0
    var connection = new WebSocket('ws://localhost:4040')
    connection.onopen = function () {
      // จะทำงานเมื่อเชื่อมต่อสำเร็จ
      // console.log('connect webSocket')
      connection.send('Hello server i"m client') // ส่ง Data ไปที่ Server
    }
    connection.onerror = function (error) {
      console.error('WebSocket Error ' + error)
    }
    connection.onmessage = function (e) {
      vm.S_plan = 0
      vm.S_actual = 0
      // log ค่าที่ถูกส่งมาจาก server
      // console.log('message from server: ', e.data)
      // console.log('parser : ', JSON.parse(e.data))
      let res = JSON.parse(e.data)
      vm.getData(res)
      vm.getRandomColor()
    }
  },
  updated () {
    // let vm = this
    // console.log('update')
    // var connection = new WebSocket('ws://localhost:4040')
    // connection.onopen = function () {

    // }
    // connection.onerror = function (error) {
    //   console.error('WebSocket Error ' + error)
    // }
    // connection.onmessage = function (e) {
    //   let res = JSON.parse(e.data)
    //   let data = res.data
    //   let sPlant = 0
    //   let sActual = 0
    //   for (var i = 0; i < data.info.option_std_time_.length; i++) {
    //     sPlant = sPlant + data.info.option_std_time_[i]
    //   }
    //   for (var j = 0; j < data.info.option_time_.length; j++) {
    //     sActual = sActual + data.info.option_time_[j]
    //   }
    // }
  },
  methods: {
    getData (res) {
      let data = res.data
      this.VIN = data.info.vin_no
      this.OPTION_GROUP = data.info.option_group
      this.start_time = data.info.start
      this.finish_time = data.info.finish
      this.diff = data.info.diff
      this.plan_time = data.info.option_std_time_
      this.actual_time = data.info.option_time_
      this.status = data.info.status
      console.log(this.status)
      // console.log(data.info.vin_no)
      // console.log(data.info.start)
      for (var i = 0; i < data.info.option_std_time_.length; i++) {
        this.S_plan = this.S_plan + data.info.option_std_time_[i]
      }
      for (var j = 0; j < data.info.option_time_.length; j++) {
        this.S_actual = this.S_actual + data.info.option_time_[j]
      }
      if (this.status === 'NO_WORKING') { // no light
        setNolight()
      } else if (this.status === 'WORKING') { //  green
        setGreen()
      } else if (this.status === 'WARNING') { //  yellow
        setYellow()
      } else if (this.status === 'DELAY') { //  red
        setRed()
      } else if (this.status === 'HELP') { //  red
        setRed()
      } else if (this.status === 'STOP_WARNING') { //  green
        setGreen()
      }
      function setRed () {
        console.log('red')
        document.getElementById('color1').style.animation = '1s step-end infinite'
        document.getElementById('color1').style.backgroundColor = '#FF0000'
        document.getElementById('color1').style.boxShadow = '0 0 6em #ff3333'

        document.getElementById('color2').style.animation = '1s step-end infinite'
        document.getElementById('color2').style.backgroundColor = '#b2b300'
        document.getElementById('color2').style.boxShadow = '0 0 0em transparent'

        document.getElementById('color3').style.animation = '1s step-end infinite'
        document.getElementById('color3').style.backgroundColor = '#00b300'
        document.getElementById('color3').style.boxShadow = '0 0 0em transparent'
      }
      function setGreen () {
        console.log('green')
        document.getElementById('color3').style.animation = '1s step-end infinite'
        document.getElementById('color3').style.backgroundColor = '#00FF00'
        document.getElementById('color3').style.boxShadow = '0 0 6em #33ff33'

        document.getElementById('color2').style.animation = '1s step-end infinite'
        document.getElementById('color2').style.backgroundColor = '#b2b300'
        document.getElementById('color2').style.boxShadow = '0 0 0em transparent'

        document.getElementById('color1').style.animation = '1s step-end infinite'
        document.getElementById('color1').style.backgroundColor = '#b30000'
        document.getElementById('color1').style.boxShadow = '0 0 0em transparent'
      }
      function setYellow () {
        console.log('yellow')
        document.getElementById('color2').style.animation = '1s step-end infinite'
        document.getElementById('color2').style.backgroundColor = '#FFFF00'
        document.getElementById('color2').style.boxShadow = '0 0 6em #ffff33'

        document.getElementById('color1').style.animation = '1s step-end infinite'
        document.getElementById('color1').style.backgroundColor = '#b30000'
        document.getElementById('color1').style.boxShadow = '0 0 0em transparent'

        document.getElementById('color3').style.animation = '1s step-end infinite'
        document.getElementById('color3').style.backgroundColor = '#00b300'
        document.getElementById('color3').style.boxShadow = '0 0 0em transparent'
      }
      function setNolight () {
        console.log('nolight')
        document.getElementById('color2').style.animation = '1s step-end infinite'
        document.getElementById('color2').style.backgroundColor = '#b2b300'
        document.getElementById('color2').style.boxShadow = '0 0 0em transparent'

        document.getElementById('color1').style.animation = '1s step-end infinite'
        document.getElementById('color1').style.backgroundColor = '#b30000'
        document.getElementById('color1').style.boxShadow = '0 0 0em transparent'

        document.getElementById('color3').style.animation = '1s step-end infinite'
        document.getElementById('color3').style.backgroundColor = '#00b300'
        document.getElementById('color3').style.boxShadow = '0 0 0em transparent'
      }
    },
    getRandomColor () {
      let letters = '0123456789ABCDEF'.split('')
      let color = '#'
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)]
      }
      // console.log(color)
      this.Pcolor = color
      return color
    },
    STOPSV () {
      console.log('stop')
      var connection = new WebSocket('ws://localhost:4040')
      connection.onopen = function () {
        // จะทำงานเมื่อเชื่อมต่อสำเร็จ
        // console.log('connect webSocket')
        connection.send(JSON.stringify({ 'protocol': 'pace_maker_status', 'data': { 'bay': 'C1', 'status': 'STOP' } })) // ส่ง Data ไปที่ Server
      }
      connection.onerror = function (error) {
        console.error('WebSocket Error ' + error)
      }
    },
    HELPSV () {
      console.log('help')
      var connection = new WebSocket('ws://localhost:4040')
      connection.onopen = function () {
        // จะทำงานเมื่อเชื่อมต่อสำเร็จ
        // console.log('connect webSocket')
        connection.send(JSON.stringify({ 'protocol': 'pace_maker_status', 'data': { 'bay': 'C1', 'status': 'HELP' } })) // ส่ง Data ไปที่ Server
      }
      connection.onerror = function (error) {
        console.error('WebSocket Error ' + error)
      }
    },
    blink () {
      let x = document.getElementById('act').value
      console.log('test change', x)
    }
  },
  directives: {
    rainbow: {
      bind (el, bind, vnode) {
        if (bind.value % 5 === 0) {
          el.style.background = '#ffc107'
        } else if (bind.value % 4 === 0) {
          el.style.background = '#28a745'
        } else if (bind.value % 3 === 0) {
          el.style.background = '#dc3545'
        } else if (bind.value % 2 === 0) {
          el.style.background = '#007bff'
        } else if (bind.value % 1 === 0) {
          el.style.background = '#6c757d'
        } else {
          el.style.background = '#343a40'
        }
        el.style.color = 'white'
      }
    }
    // blink: {
    //   bind (el, bind, vnode) {
    //     if (bind.value >= ((bind.value * 70) / 100)) {
    //       document.getElementById('color2').style.animation = '1s step-end infinite'
    //       document.getElementById('color2').style.backgroundColor = '#FFFF00'
    //       document.getElementById('color2').style.boxShadow = '0 0 6em #ffff33'

    //       document.getElementById('color1').style.animation = '1s step-end infinite'
    //       document.getElementById('color1').style.backgroundColor = '#b30000'
    //       document.getElementById('color1').style.boxShadow = '0 0 0em transparent'

    //       document.getElementById('color3').style.animation = '1s step-end infinite'
    //       document.getElementById('color3').style.backgroundColor = '#00b300'
    //       document.getElementById('color3').style.boxShadow = '0 0 0em transparent'
    //     } else if (bind.value <= ((bind.value * 70) / 100)) {
    //       document.getElementById('color3').style.animation = '1s step-end infinite'
    //       document.getElementById('color3').style.backgroundColor = '#00FF00'
    //       document.getElementById('color3').style.boxShadow = '0 0 6em #33ff33'

    //       document.getElementById('color2').style.animation = '1s step-end infinite'
    //       document.getElementById('color2').style.backgroundColor = '#b2b300'
    //       document.getElementById('color2').style.boxShadow = '0 0 0em transparent'

    //       document.getElementById('color1').style.animation = '1s step-end infinite'
    //       document.getElementById('color1').style.backgroundColor = '#b30000'
    //       document.getElementById('color1').style.boxShadow = '0 0 0em transparent'
    //     } else if (bind.value > ((bind.value * 100) / 100)) {
    //       document.getElementById('color1').style.animation = '1s step-end infinite'
    //       document.getElementById('color1').style.backgroundColor = '#FF0000'
    //       document.getElementById('color1').style.boxShadow = '0 0 6em #ff3333'

    //       document.getElementById('color2').style.animation = '1s step-end infinite'
    //       document.getElementById('color2').style.backgroundColor = '#b2b300'
    //       document.getElementById('color2').style.boxShadow = '0 0 0em transparent'

    //       document.getElementById('color3').style.animation = '1s step-end infinite'
    //       document.getElementById('color3').style.backgroundColor = '#00b300'
    //       document.getElementById('color3').style.boxShadow = '0 0 0em transparent'
    //     }
    //   }
    // }
  }
}
</script>
<style scoped>
label {
  font-size: 20px !important;
  font-family: 'Kanit', sans-serif !important;
  font-weight: bold !important;
  /* background: #a6c2ce */
}
strong {
  font-size: 28px !important;
  font-family: 'Kanit', sans-serif !important;
  font-weight: bold !important;
}
h1 {
  display: inline;
  font-family: 'Kanit', sans-serif !important;
  font-weight: bold !important;
  font-size: 3rem;
}
#P_time {
border: solid 2px #E0e0e0;
/* background-color: white; */
margin:5px;
}
#traffic-light {
  position: absolute;
  display: block;
  left: 50%;
  top: 50%;
  margin-left: -60px;
  margin-top: -160px;
  background-color: #333;
  width: 120px;
  height: 320px;
  border-radius: 30px;
}
input {
  appearance: none;
  position: relative;
  left: 50%;
  width: 80px;
  height: 80px;
  margin-top: 20px;
  margin-left: -40px;
  background-color: grey;
  /* vertical-align: middle; */
  border-radius: 100%;
  display: block;
}
input#color1 {
  background-color: #b30000;
}
input#color1:hover {
  animation: blink1 1.1s step-end infinite;
}
input#color1:checked {
  background-color: #FF0000;
  box-shadow: 0 0 6em #ff3333;
}
input#color2 {
  background-color: #b2b300;
}
input#color2:hover {
  animation: blink2 1s step-end infinite;
}
input#color2:checked {
  background-color: #FFFF00;
  box-shadow: 0 0 6em #ffff33;
}
input#color3 {
  background-color: #00b300;
}
input#color3:hover {
  animation: blink3 1s step-end infinite;
}
input#color3:checked {
  background-color: #00FF00;
  box-shadow: 0 0 6em #33ff33;
}
@keyframes blink1 {
  0% {
    background-color: #FF0000;
    box-shadow: 0 0 6em #ff3333;
  }
  50% {
    background-color: #b30000;
    box-shadow: 0 0 0em transparent;
  }
}
@keyframes blink2 {
  0% {
    background-color: #FFFF00;
    box-shadow: 0 0 6em #ffff33;
  }
  50% {
    background-color: #b2b300;
    box-shadow: 0 0 0em transparent;
  }
}
@keyframes blink3 {
  0% {
    background-color: #00FF00;
    box-shadow: 0 0 6em #33ff33;
  }
  50% {
    background-color: #00b300;
    box-shadow: 0 0 0em transparent;
  }
}
* {
  user-select: none;
  outline: none;
}
</style>
