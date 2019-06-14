<template>
  <div>
    <div id="wavesurfer" class="container">
      <div id="waveform"/>
      <div id="spectrogram"/>
    </div>
    <div id="timeline"/>
  </div>
</template>

<script>
import WaveSurfer from "wavesurfer.js";
import Spectrogram from "wavesurfer.js/dist/plugin/wavesurfer.spectrogram";
import Timeline from "wavesurfer.js/dist/plugin/wavesurfer.timeline";
import Regions from "wavesurfer.js/dist/plugin/wavesurfer.regions";

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
    };
  },

  mounted() {
    this.instance = WaveSurfer.create({
      container: "#waveform",
      pixelRatio: 1,
      height: 256,
      plugins: [
        Spectrogram.create({
          container: "#spectrogram",
          labels: true
        }),
        Timeline.create({
          container: "#timeline"
        }),
        Regions.create({})
      ]
    });

    this.instance.on("ready", () => {
      this.instance.enableDragSelection({
        color: "rgba(74, 74, 74, 0.31)"
      });
    });

    this.instance.on('loading', (e) => this.$emit('loading'));
    this.instance.on('ready', (e) => this.$emit('loaded'));
    this.instance.load(this.source);
  }
};
</script>

<style scoped>
#wavesurfer {
  height: 256px;
  position: relative;
}

#waveform,
#spectrogram {
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
