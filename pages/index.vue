<template>
  <div>
    <Post></Post>
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
    };
  },
};
</script>
