<template>
  <div class="VueToNuxtLogo">
    <p>{{ totalPercent }} %</p>
    <div
      id="parent"
      style="height: 50px; width: 600px; outline: 3px solid #F0E68C; position: relative;"
    >
      <vue-draggable-resizable
        :w="100"
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
    <div class="time">
      <span> Start : {{ startPercent }} %</span>
      <span> End : {{ endPercent }} %</span>
    </div>

    <br />
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      width: 0,
      height: 0,
      x: 0,
      y: 0,
      axis: 'x',
      a: 0,
      b: 0,
      total: 100 / 600,
      start: 0,
      end: 0
    }
  },
  computed: {
    totalPercent() {
      return parseFloat(this.total * 100).toFixed(2)
    },
    startPercent() {
      const startPercent = parseFloat(this.start * 100).toFixed(2)
      return startPercent
    },
    endPercent() {
      return parseFloat(this.end * 100).toFixed(2)
    },
    widthParent() {
      return document.getElementById('parent').clientWidth
    }
  },
  mounted() {},
  methods: {
    onResize(x, y, width, height) {
      this.x = x
      this.y = y
      this.width = width
      this.height = height

      this.a = this.widthParent - this.width
      this.b = this.widthParent - this.a
      this.total = this.b / this.widthParent

      this.start = this.x / this.widthParent
      this.end = (this.width + this.x) / this.widthParent
      console.log()
    },
    onDrag(x, y) {
      this.x = x
      this.y = y
      this.start = this.x / this.widthParent
      this.end = (this.width + this.x) / this.widthParent
    }
  }
}
</script>
<style>
.time {
  border-bottom: 1px solid black;
}

.child {
  outline: 3px solid red;
  background-color: blueviolet;
  height: 100%;
  width: 100%;
}

.handle {
  position: absolute;
  background-color: pink;
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

.handle-tl:hover,
.handle-tr:hover,
.handle-bl:hover,
.handle-br:hover {
  background-color: red;
  transform: scale(1.4);
}
</style>
