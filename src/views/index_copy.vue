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
          <!-- <b-col style="border: solid 2px #E0e0e0;background-color: #33c7f7;margin:5px;color:white;" cols="5"><br><label>URN : {{URN}}</label><br></b-col> -->
          <b-col cols="1"></b-col>
          <br>
        </b-row>
      </div>
        <!-- <label>VIN : {{ VIN }}</label><br> -->
      <!-- <div>
        <label>URN : {{URN}}</label><br>
      </div> -->
      <!-- <div>
        <b-row class="text-center">
          <b-col cols="1"></b-col>
          <b-col style="border: solid 2px #E0e0e0;background-color: #33c7f7;margin:10px;color:white;" cols="10"><br><label>OPTION GROUP : {{OPTION_GROUP}}</label><br></b-col>
          <b-col cols="1"></b-col>
        </b-row>
      </div> -->
    </div>
    <div style="text-align: left;margin-left: 10px;margin-top: 20px;">
        <div>
          <label style="border: solid 2px #E0e0e0;background-color: white;margin:5px;"><label style="margin:5px;">PLAN : <label id="s_plan">{{S_plan}}</label></label></label>
          <div>
            <b-row class="text-center">
              <b-col></b-col>
              <b-col cols="8">
                  <h1 style="border: solid 2px #E0e0e0;background-color: white;margin:5px" v-for="(plan_times, index) in plan_time" :key="index"><h1 style="margin:5px;">P{{index}} : {{plan_times}} </h1></h1>
              </b-col>
              <b-col></b-col>
            </b-row>
          </div>
        </div>
        <div>
          <!-- <label v-bind:style="S_actual < S_plan ? {color: 'green'} : S_actual > S_plan ? {color: 'bule'} : S_actual == S_plan ? { color: 'yellow'} : { color : 'black'}" -->
        <br><label style="border: solid 2px #E0e0e0;background-color: white;margin:5px;"><label style="margin:5px;"> ACTUAL : <label id="s_actual">{{S_actual}}</label></label></label>
          <div>
            <b-row class="text-center">
              <b-col></b-col>
              <b-col cols="8">
                <b-progress class="mt-2" :max="100" height="4rem" show-value  show-progress animated>
                <!-- <b-progress-bar v-for="(actual_times, index) in actual_time" :key="index"  :value="value[index]" variant="success"></b-progress-bar>v-bind:style="{'background':Pcolor}" -->
                <b-progress-bar v-for="(actual_times, index) in actual_time" :key="index" :value="actual_times" :variant="actual_times < plan_time[index] ? 'success' : actual_times > plan_time[index] ? 'danger' : actual_times == plan_time[index] ? 'warning' : 'primary'"><strong>{{actual_times}}</strong></b-progress-bar>
                </b-progress><br>
              </b-col>
              <b-col></b-col>
            </b-row>
          </div>
          <div style="display: flex;height: 100%;justify-content: center;align-items: center;">
            <b-row class="text-center">
              <b-col style="border: solid 2px #E0e0e0;background-color: #eecb27;margin:5px;color:white;" cols="3.5"><br><label style="margin:5px;">Start : {{start_time}}</label><br><br></b-col>
              <b-col style="border: solid 2px #E0e0e0;background-color: #e13239;margin:5px;color:white;" cols="3.5"><br><label style="margin:5px;">Finish : {{finish_time}}</label><br><br></b-col>
              <b-col style="border: solid 2px #E0e0e0;background-color: #1f1762;margin:5px;color:white;" cols="3.5"><br><label style="margin:5px;">Diff : {{S_plan - S_actual}}</label><br><br></b-col>
              <br>
            </b-row>
          </div>
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
      console.log('connect webSocket')
      connection.send('Hello server i"m client') // ส่ง Data ไปที่ Server
    }
    connection.onerror = function (error) {
      console.error('WebSocket Error ' + error)
    }
    connection.onmessage = function (e) {
      vm.S_plan = 0
      vm.S_actual = 0
      // log ค่าที่ถูกส่งมาจาก server
      console.log('message from server: ', e.data)
      console.log('parser : ', JSON.parse(e.data))
      let res = JSON.parse(e.data)
      vm.getData(res)
      vm.getRandomColor()
    }
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
      console.log(data.info.vin_no)
      console.log(data.info.start)
      for (var i = 0; i < data.info.option_std_time_.length; i++) {
        this.S_plan = this.S_plan + data.info.option_std_time_[i]
      }
      for (var j = 0; j < data.info.option_time_.length; j++) {
        this.S_actual = this.S_actual + data.info.option_time_[j]
      }
    },
    getRandomColor () {
      let letters = '0123456789ABCDEF'.split('')
      let color = '#'
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)]
      }
      console.log(color)
      this.Pcolor = color
      return color
    }
  }
}
</script>
<style scoped>
@import '../assets/css/vue-material.min.css';
@import '../assets/css/default.css';
label {
  font-size: 28px !important;
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
</style>
