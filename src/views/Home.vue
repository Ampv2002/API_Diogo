<template>
  <div class="home" id="app">
    <v-app id="inspire">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    </v-app>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  }
}
</script>
