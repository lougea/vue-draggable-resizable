<template>
  <div class="m-8">
    <Video :interval="interval" @videoSize="videoSize = $event" />
    <DraggableBar :dimension="dimension" @interval="interval = $event" />
  </div>
</template>

<script>
import DraggableBar from '~/components/DraggableBar.vue'
import Video from '~/components/Video.vue'

export default {
  components: {
    Video,
    DraggableBar
  },
  data() {
    return {
      interval: {},
      videoSize: 0
    }
  },
  computed: {
    dimension() {
      return { TOTAL_WIDTH: 600, INIT_WIDTH: 200, TOTAL_HEIGHT: 50 }
    }
  },
  methods: {
    // responsive_WIDTH() {
    //   if (windo)
    // }
  }
}
</script>
<style>
DraggableBAr {
  height: 50;
  width: 100%;
}
</style>
