<template>
  <section>
    <wavesurfer 
      ref="player"
      :source="source" />
    <div class="container py-2">
      <button 
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        @click="onPlay">
        {{ playPauseText }}
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
      playPauseText: 'Play',
      playText: 'Play',
      pauseText: 'Pause'
    }
  },

  methods: {
    onPlay() {
      this.playing = !this.playing
      this.$refs.player.instance.playPause()

      this.playPauseText = !this.playing ? this.playText : this.pauseText
    }
  }
};
</script>