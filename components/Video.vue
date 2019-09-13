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
      duration: 0
    }
  },
  computed: {
    startSec() {
      return parseFloat(this.interval.start * this.duration).toFixed(2)
    },
    endSec() {
      return parseFloat(this.interval.end * this.duration).toFixed(2)
    }
  },
  watch: {
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
    setDuration() {
      this.duration = this.$refs.video.duration
      this.$refs.video.currentTime = this.startSec
    },
    playPause: function() {
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
    time() {
      if (this.$refs.video.currentTime >= this.endSec) {
        this.$refs.video.pause()
      }
    },
    ended() {
      this.$refs.video.currentTime = this.startSec
    }
  }
}
</script>
