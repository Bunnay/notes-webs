<template>
  <div>
    <Post></Post>
    <!-- <Button :color="'primary'" :name="'Submit'" @submit-button="snackbar = !snackbar"></Button>
    <Snackbar :color="'primary'" :snackbar="snackbar" :text="'Posted Successfully!'"></Snackbar>
    <Modal :maxWidth="500" :myDialog="dialog" :text="'Are you sure you want to submit this form?'" :title="'Confirmation'" :titleColor="'primary--text'"></Modal> -->
    <Card></Card>
  </div>
</template>

<script>
import Modal from "../components/Modal.vue";
import Snackbar from "../components/Snackbar.vue";
import Button from "../components/Button.vue";
import Card from '../components/Card.vue';
import Post from '../components/Post.vue';
export default {
  components: { Modal, Snackbar, Button, Card, Post },
  name: "homePage",
  data() {
    return {
      dialog: false,
      snackbar: true,
      tags: ['Hello', "this is tag", 'Hi'],
      image: 'https://picsum.photos/seed/picsum/200/300'
    };
  },
};
</script>
