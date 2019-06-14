<template>
   <div id="wavesurfer" class="container">
    <div id="waveform" />
    <div id="spectrogram" />
  </div>
</template>

<script>
import WaveSurfer from 'wavesurfer.js'
import Spectrogram from 'wavesurfer.js/dist/plugin/wavesurfer.spectrogram'
import Regions from 'wavesurfer.js/dist/plugin/wavesurfer.regions'

export default {
  props: {
    source: {
      type: String,
      default: null
    }
  },

  data() {
    return {
      instance: null
    }
  },

  mounted() {
    this.instance = WaveSurfer.create({
      container: "#waveform",
      waveColor: 'violet',
      progressColor: 'purple',
      height: 256,
      plugins: [
        Spectrogram.create({
          container: '#spectrogram',
          labels: true
        })
      ]
    })

    this.instance.load(this.source)
  }
}
</script>

<style scoped>
#wavesurfer {
  position: relative;
}

#waveform, #spectrogram {
  position: absolute;
  top: 0;
  width: 100%;
}

#waveform {
  z-index: 5;
}

#waveform >>> canvas {
  display: none;
}
</style>
