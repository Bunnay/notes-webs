<template>
  <div>
    <v-overlay v-if="loading" color="#000000">
      <v-progress-circular
        indeterminate
        size="64"
        v-if="loading"
      ></v-progress-circular>
    </v-overlay>
    <div class="d-flex flex-wrap">
      <v-card
        class="box-shadow me-2 ms-2 mt-2 mb-2 flex-fill d-flex flex-column"
        width="338"
        v-for="(note, index) in notes"
        :key="index"
        outlined
      >
        <v-card-title>
          {{ note.title }}
        </v-card-title>
        <v-card-subtitle>
          {{ note.text }}
        </v-card-subtitle>
      </v-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notes: [],
      loading: false,
    };
  },

  methods: {
    async fetchNotes() {
      this.loading = true;
      const { data } = await this.$axios.get("/notes");
      this.notes = data;
      setTimeout(() => this.loading = false, 500);
    }
  },

  async fetch() {
    this.fetchNotes();
  },

  created() {
    this.$nuxt.$on("fetch-notes", () => {
      this.fetchNotes();
    });
  },

};
</script>

<style lang="scss" scoped></style>
