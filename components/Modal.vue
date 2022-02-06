<template>
  <v-dialog v-model="myDialog" :max-width="maxWidth + 'px'">
    <v-card flat>
      <v-card-title>
        <h4 :class="titleColor">{{title}}</h4>
      </v-card-title>
      <v-card-text>
        {{text}}
      </v-card-text>
      <v-card-actions>
        <Button :color="'white'" :name="'cancel'" @submit-button="$emit('click-cancel')"></Button>
        <v-spacer></v-spacer>
        <Button :color="'primary'" :name="'submit'" @submit-button="$emit('click-submit')"></Button>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
  import Button from './Button.vue';
  export default {
    components: {
      Button
    },
    props: {
      myDialog: {
        type: Boolean,
        required: false,
      },
      maxWidth: {
        type: Number,
        required: false,
      },
      title: {
        type: String,
        required: false,
      },
      titleColor: {
        type: String,
        required: false,
      },
      text: {
        type: String,
        required: false,
      },
    }
  }
</script>

<style lang="scss" scoped>

</style>
