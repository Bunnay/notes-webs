<template>
  <div>
    <Note></Note>
    <Card></Card>
  </div>
</template>

<script>
import Modal from "../components/Modal.vue";
import Snackbar from "../components/Snackbar.vue";
import Button from "../components/Button.vue";
import Card from '../components/Card.vue';
import Note from '../components/Note.vue';
export default {
  components: { Modal, Snackbar, Button, Card, Note },
  name: "homePage",
  data() {
    return {
      sheet: false,
    };
  },
};
</script>
