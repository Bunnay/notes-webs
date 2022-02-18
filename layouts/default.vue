<template>
  <v-app>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC"
      crossorigin="anonymous"
    />
    <v-app-bar app>
      <form
        no-gutters
        class="d-flex col-12 col-md-6 py-2"
        style="align-items: center"
        @submit="clickSearch"
      >
        <v-text-field
          class="me-3"
          v-model="search"
          label="Search"
          prepend-inner-icon="mdi-magnify"
          rounded
          hide-details
          solo-inverted
        >
        </v-text-field>
        <v-btn outlined rounded @click="clickSearch">Search</v-btn>
      </form>
      <v-spacer></v-spacer>
      <v-toolbar-title class="d-none d-md-flex"
        >Wall of Sharing</v-toolbar-title
      >
    </v-app-bar>
    <chip-group
      :style="{
        marginTop: '64px',
        position: 'fixed',
        zIndex: '1',
        width: '100%',
      }"
      @post-with-tags="getPostWithTags"
    ></chip-group>
    <v-main :style="{ marginTop: '60px' }">
      <v-container fluid>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import ChipGroup from "../components/ChipGroup.vue";
export default {
  components: { ChipGroup },
  name: "DefaultLayout",
  data() {
    return {
      search: "",
      posts: [],
    };
  },

  methods: {
    async clickSearch(e) {
      e.preventDefault();
      this.$nuxt.$emit("click-search", this.search);
    },

    async getPostWithTags(itemValue) {
      this.$nuxt.$emit("click-tags", itemValue);
    },
  },
};
</script>
