<template>
  <div>
    <video
      ref="video"
      src="~assets/video/video.mp4"
      @loadeddata="setDuration"
      @timeupdate="time()"
    ></video>
    <p>{{ totalSec }} seconds</p>
    <div class="flex">
      <button
        class="button play border border-bg-black flex-1/2 m-2"
        @click="videoPlay()"
      ></button>
      <div
        id="parent"
        class="flex-2/3"
        :style="
          `height: 50px; width: ${totalWidth}px; outline: 3px solid #F0E68C; position: relative;`
        "
      >
        <vue-draggable-resizable
          :w="width"
          :h="50"
          :x="0"
          :y="0"
          :axis="axis"
          :handles="['ml', 'mr']"
          :parent="true"
          :active="true"
          :prevent-deactivation="true"
          @dragging="onDrag"
          @resizing="onResize"
        >
          <div class="child"></div>
        </vue-draggable-resizable>
      </div>
    </div>

    <div class="time">
      <span> Start : {{ startSec }} seconds</span>
      <span> End : {{ endSec }}seconds</span>
    </div>
  </div>
</template>

<script>
const TOTAL_WIDTH = 400
const INIT_WIDTH = 100
export default {
  components: {},
  data() {
    return {
      width: INIT_WIDTH,
      height: 0,
      x: 0,
      y: 0,
      axis: 'x',
      total: INIT_WIDTH / TOTAL_WIDTH,
      start: 0,
      end: INIT_WIDTH / TOTAL_WIDTH,
      totalWidth: TOTAL_WIDTH,
      duration: 0,
      currentTime: 0
    }
  },
  computed: {
    totalPercent() {
      return parseFloat(this.total * 100).toFixed(2)
    },
    startPercent() {
      return parseFloat(this.start * 100).toFixed(2)
    },
    endPercent() {
      return parseFloat(this.end * 100).toFixed(2)
    },
    totalSec() {
      return parseFloat(this.total * this.duration).toFixed(2)
    },
    startSec() {
      return parseFloat(this.start * this.duration).toFixed(2)
    },
    endSec() {
      return parseFloat(this.end * this.duration).toFixed(2)
    }
  },
  mounted() {},
  methods: {
    setDuration() {
      this.duration = this.$refs.video.duration
      this.currentTime = this.$refs.video.currentTime
    },
    onResize(x, y, width, height) {
      this.x = x
      this.y = y
      this.width = width
      this.height = height
      this.total = (TOTAL_WIDTH - (TOTAL_WIDTH - this.width)) / TOTAL_WIDTH
      this.$refs.video.currentTime = this.startSec
      this.startEnd()
    },
    onDrag(x, y) {
      this.$refs.video.pause()
      this.x = x
      this.y = y
      this.$refs.video.currentTime = this.startSec
      this.startEnd()
    },
    startEnd(width) {
      this.$refs.video.pause()
      this.start = this.x / TOTAL_WIDTH
      this.end = (this.width + this.x) / TOTAL_WIDTH
    },
    videoPlay() {
      if (this.$refs.video.paused === true) {
        this.$refs.video.play()
      } else if (this.$refs.video.paused === false) {
        this.$refs.video.pause()
      }
    },
    time() {
      console.log(this.$refs.video.currentTime)
      if (this.$refs.video.currentTime >= this.endSec) {
        this.$refs.video.pause()
        this.$refs.video.currentTime = this.start
      }
    }
  }
}
</script>
<style>
.button.play {
  box-sizing: border-box;
  margin-right: 20px;
  width: 30px;
  height: 30px;
  border-style: solid;
  border-color: #202020;
  border-width: 20px 0px 20px 40px;
  border-color: transparent transparent transparent #202020;
}
.button.play:hover {
  border-color: transparent transparent transparent #f0e68c;
}

.time {
  border-bottom: 1px solid black;
}

.child {
  outline: 3px solid rgba(73, 72, 72, 0.883);
  background-color: rgba(130, 126, 134, 0.767);
  height: 100%;
  width: 100%;
}

.child:hover {
  outline: 3px solid red;
}

.handle {
  position: absolute;
  background-color: black;
  border: 1px solid black;
  border-radius: 50%;
  height: 14px;
  width: 14px;
  box-model: border-box;
  -webkit-transition: all 300ms linear;
  -ms-transition: all 300ms linear;
  transition: all 300ms linear;
}

.handle-tl {
  top: -14px;
  left: -14px;
  cursor: nw-resize;
}

.handle-tm {
  top: -14px;
  left: 50%;
  margin-left: -7px;
  cursor: n-resize;
}

.handle-tr {
  top: -14px;
  right: -14px;
  cursor: ne-resize;
}

.handle-ml {
  top: 50%;
  margin-top: -7px;
  left: -14px;
  cursor: w-resize;
}

.handle-mr {
  top: 50%;
  margin-top: -7px;
  right: -14px;
  cursor: e-resize;
}

.handle-bl {
  bottom: -14px;
  left: -14px;
  cursor: sw-resize;
}

.handle-bm {
  bottom: -14px;
  left: 50%;
  margin-left: -7px;
  cursor: s-resize;
}

.handle-br {
  bottom: -14px;
  right: -14px;
  cursor: se-resize;
}

.handle-ml:hover,
.handle-mr:hover {
  background-color: red;
  transform: scale(1.4);
}
</style>
