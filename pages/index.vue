<template>
    <v-parallax
    dark
    :src="require('@/assets/home.jpg')"
  >
    <v-row
      align="center"
      justify="center"
    >
      <v-col 
        class="text-center" 
        cols="12">
        <h1 
            class="text-style display-1 mb-4"
        >
            S-Clinic
        </h1>
        <h4 
            class="subheading text-style"
        >
            Billing software for Clinics.
        </h4>
      </v-col>
    </v-row>
  </v-parallax>
</template>
<style scoped>
  .text-style {
    color: black;
  }
</style>