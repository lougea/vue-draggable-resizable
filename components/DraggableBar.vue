<template>
  <div>
    <div
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
</template>

<script>
const TOTAL_WIDTH = 600
const INIT_WIDTH = 200
export default {
  props: {
    duration: {
      type: Number,
      default: null
    }
  },
  data: function() {
    return {
      axis: 'x',
      width: INIT_WIDTH,
      height: 0,
      x: 0,
      y: 0,
      totalWidth: TOTAL_WIDTH,
      start: 0,
      end: INIT_WIDTH / TOTAL_WIDTH
    }
  },
  mounted() {
    this.startEnd()
  },
  methods: {
    onResize: function(x, y, width, height) {
      this.x = x
      this.y = y
      this.width = width
      this.height = height
      this.total = (TOTAL_WIDTH - (TOTAL_WIDTH - width)) / TOTAL_WIDTH
      this.startEnd()
    },
    onDrag: function(x, y) {
      this.x = x
      this.y = y
      this.startEnd()
    },
    startEnd: function() {
      this.start = this.x / TOTAL_WIDTH
      this.end = (this.width + this.x) / TOTAL_WIDTH
      this.$emit('interval', {
        start: this.start,
        end: this.end,
        total: parseFloat(this.end - this.start).toFixed(5)
      })
    }
  }
}
</script>
<style>
.child {
  outline: solid 2px blue;
  height: 50px;
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
