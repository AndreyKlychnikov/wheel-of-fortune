<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
    </v-app-bar>

    <v-main>

      <FortuneWheel
        style="width: 500px"

        :canvas="canvasOptions"
        :prizes="prizes"
        :verify="cavansVerify"
        @rotateStart="onCanvasRotateStart"
        @rotateEnd="onRotateEnd"
      />
    </v-main>
  </v-app>
</template>

<script>
import FortuneWheel from 'vue-fortune-wheel'
import 'vue-fortune-wheel/lib/vue-fortune-wheel.css'

export default {
  name: 'App',

  components: {
    FortuneWheel
  },

  data: () => ({
    cavansVerify: true, // Whether the turntable in canvas mode is enabled for verification
    canvasOptions: {
      borderWidth: 6,
      borderColor: '#584b43'
    },
    prizes: [
      {
        id: 1, //* The unique id of each prize, an integer greater than 0
        name: 'Blue', // Prize name, display value when type is canvas (this parameter is not needed when type is image)
        value: 'Blue\'s value', //* Prize value, return value after spinning
        bgColor: '#45ace9', // Background color (no need for this parameter when type is image)
        color: '#ffffff', // Font color (this parameter is not required when type is image)
        probability: 30, //* Probability, up to 4 decimal places (the sum of the probabilities of all prizes
        weight: 1 // Weight, if useWeight is true, the probability is calculated by weight (weight must be an integer), so probability is invalid
      },
      {
        id: 2,
        name: 'Red',
        value: 'Red\'s value',
        bgColor: '#dd3832',
        color: '#ffffff',
        probability: 40,
        weight: 1
      },
      {
        id: 3,
        name: 'Yellow',
        value: 'Yellow\'s value',
        bgColor: '#fef151',
        color: '#ffffff',
        probability: 30,
        weight: 1
      },
      {
        id: 4,
        name: 'test',
        value: 'Yellow\'s value',
        bgColor: '#0bec0f',
        color: '#ffffff',
        probability: 30,
        weight: 1
      }
    ]
  }),
  methods: {
    onCanvasRotateStart(rotate) {
      if (this.canvasVerify) {
        const verified = true // true: the test passed the verification, false: the test failed the verification
        this.DoServiceVerify(verified, 2000).then((verifiedRes) => {
          if (verifiedRes) {
            console.log('Verification passed, start to rotate')
            rotate() // Call the callback, start spinning
            this.canvasVerify = false // Turn off verification mode
          } else {
            alert('Failed verification')
          }
        })
        return
      }
      console.log('onCanvasRotateStart')
    },
    onRotateEnd(prize) {
      alert(prize.value)
    },
    // Simulate the request back-end interface, verified: whether to pass the verification, duration: delay time
    DoServiceVerify(verified, duration) {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve(verified)
        }, duration)
      })
    }
  }
};
</script>
