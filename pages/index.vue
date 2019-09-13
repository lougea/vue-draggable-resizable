<template>
  <div class="m-8">
    <Video :interval="interval" />
    <DraggableBar @interval="interval = $event" />
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
      interval: {}
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
</style>
