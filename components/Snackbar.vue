<template>

  <div class="text-center">
    <v-snackbar
      v-model="snackbarData"
      top
      :color="color"
      :timeout="timeout"
      multi-line
    >
      <v-icon class="me-3">{{icon}}</v-icon>
      {{text}}
      <template v-slot:action="{ attrs }">
        <v-btn
          depressed
          text
          v-bind="attrs"
          @click="snackbarData = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>

</template>

<script>

  export default {
    props: {
      snackbar: {
        type: Boolean,
        required: true,
      },
      color: {
        type: String,
        required: false,
        default: "success",
      },
      timeout: {
        type: Number,
        required: false,
        default: 3000,
      },
      text: {
        type: String,
        required: false,
      },
      icon: {
        type: String,
        required: false,
      }
    },
    data() {
      return {
        snackbarData:false,
      }
    },

    watch: {
      snackbar: function(newVal, oldVal) {
        this.snackbarData = !this.snackbarDatanewVal;
      }
    }
  }

</script>

<style lang="scss" scoped>

</style>
