<template>
  <div class="container">
    <video
      ref="video"
      src="~assets/video/cold.mp4"
      @click="playPause()"
      @loadeddata="setDuration()"
      @timeupdate="time()"
      @ended="ended()"
    ></video>
    <p>Start : {{ startSec }} secs, End: {{ endSec }} secs,</p>
  </div>
</template>

<script>
export default {
  components: {},
  props: {
    interval: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      duration: 0,
      videoWidth: 0
    }
  },
  computed: {
    // Définit point de départ en secondes (string)
    startSec() {
      return parseFloat(this.interval.start * this.duration).toFixed(2)
    },
    // Définit point de fin en secondes (string)
    endSec() {
      return parseFloat(this.interval.end * this.duration).toFixed(2)
    }
  },
  watch: {
    // Watcher sur props (objet : interval), détermine image-poster lors de l'event resizing et dragging
    interval: function(value, oldValue) {
      this.$refs.video.pause()
      if (value.total === oldValue.total) {
        this.$refs.video.currentTime = this.startSec
        return
      }
      if (value.start !== oldValue.start) {
        this.$refs.video.currentTime = this.startSec
      }
      if (value.end !== oldValue.end) {
        this.$refs.video.currentTime = this.endSec
      }
    }
  },

  methods: {
    // Fonction assignée à l'Event "loadeddata" de l'élement <video>, enregistre durée vidéo
    setDuration() {
      this.duration = this.$refs.video.duration
      this.$refs.video.currentTime = this.startSec
      this.videoWidth = this.$refs.video.clientWidth
    },
    // Fonction assignée à l'Event "v-on:click" de l'élement <video>,détremine point de départ pour les actions play et pause
    playPause: function() {
      this.observer()
      if (this.$refs.video.paused === true) {
        if (this.$refs.video.currentTime >= this.endSec) {
          this.$refs.video.currentTime = this.startSec
        }
        if (this.$refs.video.currentTime < this.startSec) {
          this.$refs.video.currentTime = this.startSec
        }
        this.$refs.video.play()
      } else {
        this.$refs.video.pause()
      }
    },
    // Fonction assignée à l'Event "timeupdate" de l'élement <video>, enregistre durée vidéo, détermine point de fin et lance action pause
    time() {
      if (this.$refs.video.currentTime >= this.endSec) {
        this.$refs.video.pause()
      }
    },
    // Fonction assignée à l'Event "ended de l'élement <video>,détremine point de départ pour relancer video (si fin video atteint)
    ended() {
      this.$refs.video.currentTime = this.startSec
    },
    observer() {
      const myObserver = new ResizeObserver((entries) => {
        for (let entry of entries) {
          console.log(entry.contentRect.width)
        }
      })
      myObserver.observe(this.$refs.video)

      console.log('yo')
    }
  }
}
</script>
