<template>
  <v-btn :color="color" elevation="0" small @click="$emit('submit-button')">
    <v-icon small left v-if="icon">{{icon}}</v-icon>
    {{name}}
  </v-btn>
</template>

<script>

  export default {
    props: {
      name: {
        type: String,
        required: false,
      },
      color: {
        type: String,
        required: false,
      },
      icon: {
        type: String,
        required: false,
      },
    }

  }
</script>

<style lang="scss" scoped>

</style>
