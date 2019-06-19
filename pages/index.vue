<template>
  <section class="my-5">
    <h1 class="font-sans text-xl">Phaunos Visual Editor Example</h1>
    <wavesurfer
      :hasRegions="true"
      ref="player"
      :source="source"
      @loading="onLoading"
      @loaded="onLoaded"
      @regionClick="onRegionClick"
      @regionUpdated="onRegionUpdated"
      @regionRemoved="onRegionRemoved"
    />
    <div class="container py-2">
      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        @click="onPlay"
      >{{ actionButton }}</button>
    </div>
    <div class="container">
      <input placeholder="Start time" name="startTime" v-model="startTime">
      <input placeholder="End time" name="endTime" v-model="endTime">
      <textarea placeholder="Note" v-model="description"/>
    </div>
  </section>
</template>

<script>
import Wavesurfer from "~/components/Wavesurfer.vue";

export default {
  components: {
    Wavesurfer
  },

  data() {
    return {
      source: "audio.mp3",
      playing: false,
      actionButton: "Loading...",
      playText: "Play",
      pauseText: "Pause",
      startTime: null,
      endTime: null,
      description: "",
      currentRegion: null
    };
  },

  watch: {
    description(value) {
      if (this.currentRegion && value) {
        const region = this.$refs.player.instance.regions.list[
          this.currentRegion
        ];
        region.data.description = value;
        region.element.querySelector(".region-title").innerText = value;
      }
    }
  },

  methods: {
    onPlay() {
      this.playing = !this.playing;
      this.$refs.player.instance.playPause();

      this.actionButton = !this.playing ? this.playText : this.pauseText;
    },

    onLoading() {
      this.actionButton = "Loading...";
    },

    onLoaded() {
      this.actionButton = this.playText;
    },

    onRegionClick(e) {
      if (!e || !e.element) {
        this.currentRegion = null
        return
      }

      this.startTime = Math.round(e.start);
      this.endTime = Math.round(e.end);
      this.description = e.data.description;
      this.currentRegion = e.id
    },

    onRegionUpdated(e) {
      if (!e || !e.element) {
        this.currentRegion = null
        return
      }

      this.startTime = Math.round(e.start);
      this.endTime = Math.round(e.end);
      this.description = e.data.description;
      this.currentRegion = e.id
    },

    onRegionRemoved() {
      this.currentRegion = null;
      this.description = null
      this.startTime = null
      this.endTime = null
    }
  }
};
</script>