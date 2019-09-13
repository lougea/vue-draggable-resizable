<template>
  <div class="m-8">
    <Video @duration="duration = $event" @currentTime="currenTime = $event" />
    <DraggableBar :duration="duration" :current-time="currentTime" />
    <button @click="test()">test</button>
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
      duration: 0,
      currentTime: 0
    }
  },
  methods: {
    test() {
      console.log(this.duration)
      console.log(this.currentTime)
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
