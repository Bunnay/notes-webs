<template>
  <div>
    <Post></Post>
    <Card></Card>
    <!-- <bottom-sheet></bottom-sheet> -->
  </div>
</template>

<script>
import Modal from "../components/Modal.vue";
import Snackbar from "../components/Snackbar.vue";
import Button from "../components/Button.vue";
import Card from '../components/Card.vue';
import Post from '../components/Post.vue';
import BottomSheet from '../components/BottomSheet.vue';
export default {
  components: { Modal, Snackbar, Button, Card, Post, BottomSheet },
  name: "homePage",
  data() {
    return {
      sheet: false,
    };
  },
};
</script>
