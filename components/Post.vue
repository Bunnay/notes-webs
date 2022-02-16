<template>
  <div>
    <v-container fluid>
    <v-card outlined>
      <v-card-title>
        <h5>Create Post</h5>
      </v-card-title>
      <v-card-text>
        <v-btn
          large
          left
          block
          class="rounded"
          @click="(dialog = true), clickPostButton()"
          outlined
          color="teal"
        >
          What is on your mind, Bunnay?
        </v-btn>
      </v-card-text>
    </v-card></v-container>

    <Modal
      :maxWidth="500"
      :myDialog="dialog"
      :title="'Create Post'"
      :loading="loading"
      :titleColor="'primary--text'"
      @click-cancel="dialog = false"
      @click-submit="submitPost()"
    >
      <template v-slot:modalContent>
        <v-text-field
          label="Title"
          filled
          v-model="title"
          class="rounded mb-2"
          dense
          clearable
          hide-details
        >
        </v-text-field>

        <v-textarea
          label="Body"
          filled
          v-model="body"
          class="rounded mb-2"
          dense
          clearable
          height="100"
          hide-details
        >
        </v-textarea>

        <!-- <v-btn block elevation="0" class="rounded">
          <v-icon>mdi-image</v-icon>
          Add Image</v-btn
        > -->
        <v-file-input label="Image" v-model="image" filled class="rounded" type="file" accept="image/x-png,image/gif,image/jpeg">

        </v-file-input>
      </template>
    </Modal>

    <Snackbar
      :color="success ? 'primary' : 'red'"
      :snackbar="snackbar"
      :text="success ? success : error"
      :icon="success ? 'mdi-user' : 'mdi-user'"
    ></Snackbar>
  </div>
</template>

<script>
import Modal from "./Modal.vue";
import Snackbar from "./Snackbar.vue";
export default {
  components: { Modal, Snackbar },
  data() {
    return {
      dialog: false,
      title: "",
      body: "",
      image: null,
      tag_ids: [1, 2],
      loading: false,
      success: null,
      error: null,
      snackbar: false,
      posts: [],
    };
  },

  methods: {
    clickPostButton() {
      this.title = "";
      this.body = "";
      this.image = null;
      this.tag_ids = [1, 2];
    },

    async submitPost() {
      this.success = null;
      this.error = null;
      this.loading = true;
      try {
        // const hasImage = {
        //   image: this.image,
        // };
        // const withoutImage = {
        //   body: this.body,
        //   title: this.title,
        //   tag_ids: this.tag_ids,
        // };
        // const res = Object.assign(withoutImage, this.image ? hasImage : "");
        const formData = new FormData();
        formData.append('title', this.title)
        formData.append('body', this.body)
        for(let i=0;i<this.tag_ids.length;i++) {
            formData.append(`tag_ids[${i}]`, this.tag_ids[i])
        };
        formData.append('image', this.image);
        let response = await this.$axios.post("/posts", formData);
        this.success = response.data.message;
      } catch (e) {
        this.error = e.response.data.message;
      } finally {
        this.dialog = false;
        this.loading = false;
        this.snackbar = true;
        this.$fetch();
      }
      setTimeout(() => {
        this.snackbar = false;
      }, 1000);
    },
  },

  async fetch() {
    const { data } = await this.$axios.get(
      "/posts?sort=-created_at&include=tags"
    );
    this.posts = data.data;
  },
};
</script>

<style lang="scss" scoped>
::v-deep .v-input__slot::before {
  border-style: none !important;
}
</style>
