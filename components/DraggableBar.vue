<template>
  <div>
    <div
      :style="
        `height: ${totalHeight}px; width: ${totalWidth}px; outline: 3px solid #F0E68C; position: relative; margin:10px`
      "
    >
      <vue-draggable-resizable
        :w="width"
        :h="totalHeight"
        :x="x"
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
export default {
  props: {
    duration: {
      type: Number,
      default: null
    },
    dimension: {
      type: Object,
      default: null
    }
  },
  data: function() {
    return {
      width: this.dimension.INIT_WIDTH,
      totalWidth: this.dimension.TOTAL_WIDTH,
      totalHeight: this.dimension.TOTAL_HEIGHT,
      axis: 'x',
      x: 0,
      start: 0,
      end: this.dimension.INIT_WIDTH / this.dimension.TOTAL_WIDTH
    }
  },
  mounted() {
    this.startEnd()
  },
  methods: {
    // Fonction assignée à l'Event "resizing" (module : vue-draggable-resizable / installé en pluging)
    onResize: function(x, _, width) {
      this.x = x
      this.width = width
      this.total =
        (this.dimension.TOTAL_WIDTH - (this.dimension.TOTAL_WIDTH - width)) /
        this.dimension.TOTAL_WIDTH
      this.startEnd()
    },
    //Fonction assignée à l'Event "dragging" (module : vue-draggable-resizable / installé en pluging)
    onDrag: function(x) {
      this.x = x
      this.startEnd()
    },
    // Détermine point de départ "start", point de fin "end" et envoyer object interval ("start", "end", "total")
    startEnd: function() {
      this.start = this.x / this.dimension.TOTAL_WIDTH
      this.end = (this.width + this.x) / this.dimension.TOTAL_WIDTH
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

  -webkit-transition: all 300ms linear;
  -ms-transition: all 300ms linear;
  transition: all 300ms linear;
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

.handle-tl:hover,
.handle-tr:hover,
.handle-bl:hover,
.handle-br:hover {
  background-color: red;
  transform: scale(1.4);
}
</style>
