/* eslint-disable */
<template>
  <div id="app" style="text-align: center">
    <div style="border-radius: 10px; border-width: 1px">
      <div>
        Sensor Permissions: {{permissionsResult}}
      </div>
      <div>
        Sensor Permissions Error: {{permissionsError}}
      </div>
      <div>
        Sensor Permissions State: {{permissionsResult && permissionsResult.state}}
      </div>
      <div>
        window.Accelerometer: {{getWindow().Accelerometer}}
      </div>
      <div>
        accelerometer: {{accelerometer}}
      </div>
      <div>
        accelerometerReadDate: {{accelerometerReadDate}}
      </div>
      <div>
        x: {{accelerometerX}}
      </div>
      <div>
        y: {{accelerometerY}}
      </div>
      <div>
        z: {{accelerometerZ}}
      </div>
      <div>
        accelerometerError: {{accelerometerError}}
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
    return {
      permissionsResult: null,
      permissionsError: null,
      accelerometer: null,
      accelerometerData: {},
      accelerometerX: 0,
      accelerometerY: 0,
      accelerometerZ: 0,
      accelerometerReadDate: null,
      accelerometerError: null,
    }
  },
  methods: {
    getWindow() {
      return window
    },
    initAccelerometer() {
      try {
        this.accelerometer = new Accelerometer({frequency: 10})
        this.accelerometer.addEventListener('reading', () => {
          this.accelerometerData = {...this.accelerometer}
          this.accelerometerX = this.accelerometer.x
          this.accelerometerY = this.accelerometer.y
          this.accelerometerZ = this.accelerometer.z
          this.accelerometerReadDate = new Date().toISOString()
          console.log(`${new Date().toISOString()}: got accelerometer reading: `, this.accelerometerData)
        })
        this.accelerometer.start()
      } catch(error) {
        this.accelerometerError = error
      }
    },
    async requestPermissions() {
      this.permissionsResult = await navigator.permissions.query({name: "accelerometer"})
      if(this.permissionsResult.state === 'denied') {
        return
      }
      this.initAccelerometer()
    }
  },
  mounted() {
    console.log("mounted")
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
