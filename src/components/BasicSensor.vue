<template>
  <div style="border-radius: 10px; border: 1px solid gray; margin: 8px; padding: 8px">
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
    <div v-if="sensorError">
      Sensor Error: {{sensorError}}
    </div>

    <div>
      Downward Vector
    </div>
    <svg viewBox="0 0 100 100" style="width: 100px; height: 100px;">
      <circle r="50" cx="50" cy="50" fill="white" stroke="black" ></circle>
      <line x1="50" y1="50" :x2="sensorVectorAtCircle.x" :y2="sensorVectorAtCircle.y" stroke="green"></line>
      <circle :r="arrowRadiusAtCircle" :cx="sensorVectorAtCircle.x" :cy="sensorVectorAtCircle.y" fill="green" />
    </svg>

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
    computed: {
      sensorVectorAtCircle() {
        const normalized = this.sensorVectorNormalizedTo50()
        normalized.x += 50
        normalized.y *= -1 // for svg coords
        normalized.y += 50
        normalized.z += 50
        return normalized
      },
      arrowRadiusAtCircle() {
        return ((this.sensorVectorNormalized().z + 1) / 2) * 4 + 1
      }
    },
    methods: {
      sensorVectorNormalized() {
        const magnitude = Math.hypot(this.x, this.y, this.z)
        return {
          x: this.x / magnitude,
          y: this.y / magnitude,
          z: this.z / magnitude
        }
      },
      sensorVectorNormalizedTo50() {
        const normalized = this.sensorVectorNormalized()
        normalized.x *= 50
        normalized.y *= 50
        normalized.z *= 50
        return normalized
      },
      formatSensorDouble(double) {
        return double.toFixed(3)
      },
      initBasicSensor(SensorClass, referenceFrame = "screen") {
        try {
          this.sensor = new SensorClass({
            frequency: 60,
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
