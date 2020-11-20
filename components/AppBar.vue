<template>
  <div>
    <v-app-bar
      id="home-app-bar"
      app
      color="white"
      elevation="1"
      height="80"
    >
      <v-img
        :src="require('@/assets/logo.svg')"
        class="mr-3 hidden-xs-only"
        contain
        max-width="52"
        width="100%"
      />

      <v-img
        :src="require('@/assets/zero-logo-light.svg')"
        contain
        max-width="128"
        width="100%"
      />

      <v-spacer />

      <div>
        <v-tabs
        >
          <v-tab
            v-for="(item, i) in items"
            :key="i"
            router
            :to="item.to"
            text
          >
            {{ item.title }}
          </v-tab>
        </v-tabs>
      </div>
    </v-app-bar>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        drawer: null,
        items: [
          { title: 'Home', to: '/' },
          { title: 'Contact Us', to: '/contact-us' },
          { title: 'Login', to: '/login' }
        ]
      }
    }
  }
</script>