/* eslint-disable */
<template>
  <div id="app" style="text-align: center">
    <div style="border-radius: 10px; border-width: 1px">
      <div>
        Accelerometer:
      </div>
      <div>
        x: {{accelerometerData.x}}
      </div>
      <div>
        y: {{accelerometerData.y}}
      </div>
      <div>
        z: {{accelerometerData.z}}
      </div>
      <div>
        Error: {{accelerometerError}}
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
      accelerometer: null,
      accelerometerData: {},
      accelerometerError: null,
    }
  },
  methods: {
    initAccelerometer() {
      try {
        this.accelerometer = new Accelerometer({frequency: 10})
        this.accelerometer.addEventListener('reading', () => {
          this.accelerometerData = {...this.accelerometer}
        })
      } catch(error) {
        this.accelerometerError = error
      }
    }
  },
  mounted() {
    console.log("test")
    this.initAccelerometer()
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
