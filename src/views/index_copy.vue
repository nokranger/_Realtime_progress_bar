<template>
<div>
    <meta charset="utf-8">
    <meta content="width=device-width,initial-scale=1,minimal-ui" name="viewport">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700,400italic|Material+Icons">
    <link rel="stylesheet" href="https://unpkg.com/vue-material/dist/vue-material.min.css">
    <link rel="stylesheet" href="https://unpkg.com/vue-material/dist/theme/default.css">
    <!-- <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.dev.js"> -->
    <div style="text-align: left;margin-left: 10px;margin-top: 20px;">
      <div>
        <!-- <div>
          plan, actual
          <ul>
            <li v-for="(actual_times,indexss) in actual_time" :key="indexss">{{indexss}}-----{{ actual_times }}</li><br>
            <li>{{plan_time[0] + plan_time[1] + plan_time[2]}}</li>
            <li>{{actual_time[0] + actual_time[1] + actual_time[2]}}</li>
          </ul>
        </div> -->
        <label>VIN : {{ VIN }}</label><br>
      </div>
      <div>
        <label>URN : {{URN}}</label><br>
      </div>
      <div>
        <label>OPTION GROUP : {{OPTION_GROUP}}</label><br>
      </div>
    </div>
    <div style="text-align: left;margin-left: 10px;margin-top: 20px;">
        <div>
          <label>PLAN : {{plan_time[0] + plan_time[1] + plan_time[2]}}</label>
          <div>
            <b-row class="text-center">
              <b-col></b-col>
              <b-col cols="8">
              <!-- <b-progress :value="plan_time[0] + plan_time[1] + plan_time[2]" :max="max" show-progress animated></b-progress><br> -->
                <b-progress class="mt-2" :max="100" show-value  show-progress animated>
                <!-- <b-progress-bar v-for="(actual_times, index) in actual_time" :key="index"  :value="value[index]" variant="success"></b-progress-bar> -->
                <b-progress-bar :value="plan_time[0]" variant="success"></b-progress-bar>
                <b-progress-bar :value="plan_time[1]" variant="warning"></b-progress-bar>
                <b-progress-bar :value="plan_time[2]" variant="danger"></b-progress-bar>
                </b-progress><br>
              </b-col>
              <b-col></b-col>
            </b-row>
          </div>
        </div>
        <div>
        <label>ACTUAL : {{actual_time[0] + actual_time[1] + actual_time[2]}}</label>
          <div>
            <b-row class="text-center">
              <b-col></b-col>
              <b-col cols="8">
                <b-progress class="mt-2" :max="100" show-value  show-progress animated>
                <!-- <b-progress-bar v-for="(actual_times, index) in actual_time" :key="index"  :value="value[index]" variant="success"></b-progress-bar> -->
                <b-progress-bar :value="actual_time[0]" variant="success"></b-progress-bar>
                <b-progress-bar :value="actual_time[1]" variant="warning"></b-progress-bar>
                <b-progress-bar :value="actual_time[2]" variant="danger"></b-progress-bar>
                </b-progress><br>
              </b-col>
              <b-col></b-col>
            </b-row>
            <!-- <button v-on:click="rendomValue ()">Random</button> -->
          </div>
          <div>
            <label>Start : {{start_time}}</label><br>
          </div>
          <div>
            <label>Finish : {{finish_time}}</label><br>
          </div>
          <div>
            <label>Diff : {{diff}}</label><br>
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
      counter: 0
    }
  },
  created () {
    let vm = this
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
      // log ค่าที่ถูกส่งมาจาก server
      console.log('message from server: ', e.data)
      console.log('parser : ', JSON.parse(e.data))
      let res = JSON.parse(e.data)
      vm.getData(res)
    }
  },
  methods: {
    rendomValue () {
      // socket.emit('value', 'bbbb')
      // this.value = Math.random() * this.aa
    },
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

      // this.VIN = data.info.vin_no
    }
  }
}
</script>
<style scoped>

</style>
