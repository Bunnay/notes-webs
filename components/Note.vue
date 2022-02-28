<template>
  <div>
    <v-container fluid class="my-4">
      <v-card class="box-shadow">
        <v-card-title>
          <h5 class="primary--text">Create Note</h5>
        </v-card-title>
        <v-card-text>
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
          label="Text"
          filled
          v-model="text"
          class="rounded mb-2"
          dense
          clearable
          height="100"
          hide-details
        >
        </v-textarea></v-card-text>
        <v-card-actions>
          <v-btn depressed color="primary" block @click="submitPost">Submit</v-btn>
        </v-card-actions>
        </v-card>

    </v-container>

    <Snackbar
      :color="success ? 'cyan' : 'red'"
      :snackbar="snackbar"
      :text="success ? success : error"
      :icon="success ? 'mdi-check-circle' : 'mdi-alert-circle'"
    ></Snackbar>

  </div>
</template>

<script>
import Snackbar from '../components/Snackbar.vue';
export default {
  components: {Snackbar},
  data() {
    return {
      title: "",
      loading: false,
      text: "",
      success: null,
      error: null,
      snackbar: false,
    };
  },

  methods: {

    async submitPost() {
      this.loading = true;
      this.success = null;
      this.error = null;
      try {
        let response = await this.$axios.post("/notes", {
          title: this.title,
          text: this.text,
        });
        this.success = 'Created Successfully!';
        this.title = '';
        this.text = '';
      } catch (e) {
        this.error = 'Fail to create note!';
      } finally {
        this.snackbar = true;
        this.loading = false;
        this.fetchNotes();
      }
      setTimeout(() => {
        this.snackbar = false;
      }, 1000);
    },

    async fetchNotes() {
      this.$nuxt.$emit("fetch-notes");
    },
  },

};
</script>

<style lang="scss" scoped>
::v-deep .v-input__slot::before {
  border-style: none !important;
}
</style>
