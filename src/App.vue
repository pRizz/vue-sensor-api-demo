/* eslint-disable */
<template>
  <div id="app" style="text-align: center">
    <div style="border-radius: 10px; border-width: 1px; border-color: gray; margin: 8px;">
      <div>
        Sensor Permissions: {{permissionsResult}}
      </div>
      <div>
        Sensor Permissions Error: {{permissionsError}}
      </div>
      <div>
        Sensor Permissions State: {{permissionsResult && permissionsResult.state}}
      </div>

      <div v-if="havePermissions">
        <BasicSensor :SensorClass="getWindow().Accelerometer" referenceFrame="device" title="Accelerometer, device reference frame" />
        <BasicSensor :SensorClass="getWindow().Accelerometer" referenceFrame="screen" title="Accelerometer, screen reference frame" />

        <!--      GravitySensor is not well implemented in browsers yet -->

        <!--      <BasicSensor :SensorClass="getWindow().GravitySensor" referenceFrame="device" title="Accelerometer, device reference frame" />-->
        <!--      <BasicSensor :SensorClass="getWindow().GravitySensor" referenceFrame="device" title="Accelerometer, device reference frame" />-->

      </div>

    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import BasicSensor from './components/BasicSensor'

export default {
  name: 'app',
  components: {
    BasicSensor,
    HelloWorld
  },
  data() {
    return {
      permissionsResult: null,
      permissionsError: null,
      havePermissions: false
    }
  },
  methods: {
    getWindow() {
      return window
    },
    formatSensorDouble(double) {
      return double.toFixed(3)
    },
    async requestPermissions() {
      this.permissionsResult = await navigator.permissions.query({name: "accelerometer"})
      if(this.permissionsResult.state === 'denied') {
        this.havePermissions = false
        return
      }
      this.havePermissions = true
    }
  },
  mounted() {
    this.requestPermissions().catch((error) => {
      this.permissionsError = error
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
