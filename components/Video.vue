<template>
  <div class="container">
    <video
      ref="video"
      src="~assets/video/cold.mp4"
      @click="playPause()"
      @loadeddata="setDuration"
    ></video>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      duration: 0,
      currentTime: 0
    }
  },
  methods: {
    setDuration() {
      this.duration = this.$refs.video.duration
      this.currentTime = this.$refs.video.currentTime
      this.$emit('duration', this.duration)
      this.$emit('currentTime', this.currentTime)
    },
    playPause: function() {
      if (this.$refs.video.paused === true) {
        this.$refs.video.play()
        console.log(this.currentTime)
      } else {
        this.$refs.video.pause()
      }
    }
  }
}
</script>
