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
    },

    hasRegions: {
      type: Boolean,
      default: false
    }
  },

  data() {
    return {
      instance: null
    };
  },

  mounted() {
    const plugins = [
      Spectrogram.create({
        container: "#spectrogram",
        labels: true
      }),
      Timeline.create({
        container: "#timeline"
      })
    ];

    if (this.hasRegions) {
      plugins.push(Regions.create({}));
    }

    this.instance = WaveSurfer.create({
      container: "#waveform",
      pixelRatio: 1,
      height: 256,
      plugins: plugins
    });

    if (this.hasRegions) {
      this.instance.on("ready", () => {
        this.instance.enableDragSelection({
          color: "rgba(74, 74, 74, 0.31)"
        });
      });

      this.instance.on("region-created", e => {
        this.onRegionCreated(e);
        this.$emit("regionCreated", e);
      });

      this.instance.on("region-click", e => this.$emit("regionClick", e));
      this.instance.on("region-updated", e => this.$emit("regionUpdated", e));
      this.instance.on("region-removed", e => this.$emit("regionRemoved", e));
    }

    this.instance.on("loading", e => this.$emit("loading"));
    this.instance.on("ready", e => this.$emit("loaded"));
    this.instance.load(this.source);
  },

  methods: {
    onRegionCreated(region) {
      const close = document.createElement("div");

      close.addEventListener("click", (e) => {
        e.preventDefault();
        region.remove();
      });

      close.classList.add("region-close");
      close.style.border = "1px solid red";
      close.style.borderRadius = "50%";
      close.style.width = "10px";
      close.style.background = "red";
      close.style.height = "10px";
      close.style.position = "absolute";
      close.style.right = 0;
      close.style.margin = "5px";
      close.style.zIndex = 100;

      region.element.appendChild(close);

      const title = document.createElement("div");

      title.classList.add("region-title");

      region.element.appendChild(title);
    }
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

#waveform >>> .region-title {
  margin: 0 auto;
  height: 100%;
  color: #fff;
  writing-mode: vertical-rl;
  text-align: center;
}
</style>
