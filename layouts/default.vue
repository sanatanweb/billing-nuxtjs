<template>
  <v-app>
    <app-bar />
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <Footer />
  </v-app>
</template>
<script>
  import AppBar from '@/components/AppBar.vue'
  import Footer from '@/components/Footer.vue'

  export default {
    components: {
      AppBar,
      Footer,
    },
  
  }
</script>