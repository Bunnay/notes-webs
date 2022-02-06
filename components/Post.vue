<template>
  <div>
    <v-btn block large left class="mb-5" @click="dialog = true">
      What is on your mind, Bunnay?
    </v-btn>
    <Modal :maxWidth="500" :myDialog="dialog" :text="'Are you sure you want to submit this form?'" :title="'Confirmation'" :titleColor="'primary--text'" @click-cancel="dialog = false" @click-submit="dialog = true"></Modal>
  </div>
</template>

<script>
  import Modal from './Modal.vue';
  export default {
    components: {Modal},
    data() {
      return {
        dialog: false,
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>
