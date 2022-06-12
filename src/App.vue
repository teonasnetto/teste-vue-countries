<template>
  <div id="app" class="min-h-screen bg-gray-900">
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav> -->
    <NavHeader />
    <router-view/>
  </div>
</template>

<script>
import NavHeader from "@/components/NavHeader.vue"
export default {
  name: 'App',
  components: {
    NavHeader,
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  @apply text-gray-200;
}
</style>
