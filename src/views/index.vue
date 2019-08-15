<template>
<div>
    <meta charset="utf-8">
    <meta content="width=device-width,initial-scale=1,minimal-ui" name="viewport">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700,400italic|Material+Icons">
    <link rel="stylesheet" href="https://unpkg.com/vue-material/dist/vue-material.min.css">
    <link rel="stylesheet" href="https://unpkg.com/vue-material/dist/theme/default.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.2.0/socket.io.dev.js">
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
import io from 'socket.io-client'
let socket = io('http://localhost:4000')
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
      max: 0
    }
  },
  created () {
    console.log('define value is : ', this.value)
    // socket.emit('value', 'VIN, URN, OPGROUP, value, MAX')
    // socket.on('value', (data) => {
    //   console.log('from server data is : ', data)
    //   this.VIN = data.a
    //   this.URN = data.b
    //   this.OPTION_GROUP = data.c
    //   this.value = data.e
    //   console.log('this is value : ', this.value)
    // })
    socket.emit('test', 'TEST from client')
    socket.on('test', (data) => {
      this.max_plan_time = 0
      console.log('max plan reset : ', this.max_plan_time)
      this.max_actual_time = 0
      console.log('max actual reset: ', this.max_actual_time)
      // console.log('from server test data is : ', data)
      console.log('from server test data2 is : ', data.data)
      // console.log('from server test info is : ', data.data.info)
      // console.log('from server test urn is : ', data.data.info.urn_no)
      this.URN = data.data.info.urn_no
      // console.log('from server test vin is : ', data.data.info.vin_no)
      this.VIN = data.data.info.vin_no
      // console.log('from server test op is : ', data.data.info.option_group)
      this.OPTION_GROUP = data.data.info.option_group
      this.start_time = data.data.info.start
      this.finish_time = data.data.info.finish
      this.diff = data.data.info.diff
      for (let i = 0; i < 3; i++) {
        // console.log('from server test plan is : ', data.data.info.option_std_time_[i])
        this.plan_time[i] = data.data.info.option_std_time_[i]
        // console.log('plan time : ', this.plan_time)
        // this.max_plan_time = this.max_plan_time + this.plan_time[i]
        // console.log('max plan : ', this.max_plan_time)
      }
      // console.log('this type of plan time', typeof(this.plan_time))
      for (let i = 0; i < 3; i++) {
        // console.log('from server test actual is : ', data.data.info.option_time_[i])
        this.actual_time[i] = data.data.info.option_time_[i]
        // this.value = this.actual_time[i]
        // console.log('actual time : ', this.actual_time)
      }
      this.max = (this.plan_time[0] + this.plan_time[1] + this.plan_time[2])
    })
  },
  methods: {
    rendomValue () {
      socket.emit('value', 'bbbb')
      // this.value = Math.random() * this.aa
    }
  }
}
</script>
<style scoped>

</style>
