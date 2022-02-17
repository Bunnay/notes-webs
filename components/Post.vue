<template>
  <div>
    <v-container fluid class="my-4">
      <v-btn
        large
        left
        block
        class="rounded"
        @click="(dialog = true), clickPostButton()"
        outlined
        color="primary"
      >
        What is on your mind?
      </v-btn>
      <!-- <v-card outlined>
        <v-card-title>
          <h5>Let the world know </h5>
        </v-card-title>
        <v-card-text>
          
        </v-card-text>
      </v-card> -->
    </v-container>

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

        <v-autocomplete
          v-model="tag_ids"
          item-text="name"
          item-value="id"
          :items="tags"
          multiple
          label="Tags"
          small-chips
          deletable-chips
          full-width
          hide-details
          filled
          class="rounded mb-2"
        >
        </v-autocomplete>
        <v-file-input
          label="Image"
          v-model="image"
          filled
          class="rounded"
          type="file"
          accept="image/x-png,image/gif,image/jpeg"
          prepend-icon=""
        >
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
      tag_ids: [],
      loading: false,
      success: null,
      error: null,
      snackbar: false,
      posts: [],
      tags: [],
    };
  },

  methods: {
    clickPostButton() {
      this.title = "";
      this.body = "";
      this.image = null;
      this.tag_ids = [];
    },

    async submitPost() {
      this.success = null;
      this.error = null;
      this.loading = true;
      try {
        const formData = new FormData();
        formData.append("title", this.title);
        formData.append("body", this.body);
        for (let i = 0; i < this.tag_ids.length; i++) {
          formData.append(`tag_ids[${i}]`, this.tag_ids[i]);
        }
        this.image ? formData.append("image", this.image) : "";
        let response = await this.$axios.post("/posts", formData);
        this.success = response.data.message;
      } catch (e) {
        this.error = e.response.data.message;
      } finally {
        this.dialog = false;
        this.loading = false;
        this.snackbar = true;
        this.fetchPosts();
      }
      setTimeout(() => {
        this.snackbar = false;
      }, 1000);
    },
    async getTags() {
      const { data } = await this.$axios.get("/tags?limit=*");
      this.tags = data.data;
    },

    async fetchPosts() {
      this.$nuxt.$emit("fetch-posts", this.posts);
    },
  },

  async fetch() {
    const { data } = await this.$axios.get(
      "/posts?sort=-created_at&include=tags&limit=100"
    );
    this.posts = data.data;
    this.getTags();
  },
};
</script>

<style lang="scss" scoped>
::v-deep .v-input__slot::before {
  border-style: none !important;
}
</style>
