<template>
  <div>
    <main-game></main-game>
  </div>
</template>

<script>
// @ is an alias to /src
import MainGame from '@/components/MainGame.vue'

export default {
  name: 'Home',
  components: {
    MainGame
  }
}
</script>
