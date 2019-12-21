<template>
  <div style="border-radius: 10px; border: 1px solid gray; margin: 8px;">
    <h3>
      {{title}}
    </h3>
    <div>
      sensor: {{sensor}}
    </div>
    <div>
      sensor read date: {{readDate}}
    </div>
    <div>
      x: {{x}}
    </div>
    <div>
      y: {{y}}
    </div>
    <div>
      z: {{z}}
    </div>
    <div>
      Sensor Error: {{sensorError}}
    </div>
  </div></template>

<script>
  export default {
    name: "BasicSensor",
    props: ["SensorClass", "referenceFrame", "title"],
    data() {
      return {
        x: 0,
        y: 0,
        z: 0,
        readDate: null,
        sensor: null,
        sensorError: null
      }
    },
    methods: {
      formatSensorDouble(double) {
        return double.toFixed(3)
      },
      initBasicSensor(SensorClass, referenceFrame = "screen") {
        try {
          this.sensor = new SensorClass({
            frequency: 10,
            referenceFrame
          })
          this.sensor.addEventListener('reading', () => {
            this.x = this.formatSensorDouble(this.sensor.x)
            this.y = this.formatSensorDouble(this.sensor.y)
            this.z = this.formatSensorDouble(this.sensor.z)
            this.readDate = new Date().toISOString()
          })
          this.sensor.start()
        } catch(error) {
          this.sensorError = error
        }
      }
    },
    mounted() {
      console.log("sensor class", this.SensorClass)
      this.initBasicSensor(this.SensorClass, this.referenceFrame)
    }
  }
</script>

<style scoped>

</style>
