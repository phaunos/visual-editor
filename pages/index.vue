<template>
  <section class="my-5">
    <h1 class="font-sans text-xl">Phaunos Visual Editor Example</h1>
    <wavesurfer 
      ref="player"
      :source="source"
      @loading="onLoading"
      @loaded="onLoaded" />
    <div class="container py-2">
      <button 
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        @click="onPlay">
        {{ actionButton }}
      </button>
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
      source: 'audio.mp3',
      playing: false,
      actionButton: 'Loading...',
      playText: 'Play',
      pauseText: 'Pause'
    }
  },

  methods: {
    onPlay() {
      this.playing = !this.playing
      this.$refs.player.instance.playPause()

      this.actionButton = !this.playing 
        ? this.playText 
        : this.pauseText
    },

    onLoading() {
      this.actionButton = 'Loading...'
    },

    onLoaded() {
      this.actionButton = this.playText
    }
  }
};
</script>