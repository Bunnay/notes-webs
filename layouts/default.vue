<template>
  <v-app>
    <v-app-bar app>
      <v-avatar
        ><v-img :src="require('~/assets/image/profile.png')"></v-img
      ></v-avatar>
      <v-row no-gutters>
        <v-col cols="10">
          <v-text-field
            class="mx-1 ms-4"
            v-model="search"
            label="Search"
            prepend-inner-icon="mdi-magnify"
            rounded
            hide-details
            solo-inverted
          >
          </v-text-field>
        </v-col>
        <v-col>
          <v-btn outlined rounded class="mt-2" @click="clickSearch">Search</v-btn>
        </v-col>
      </v-row>
      <v-spacer></v-spacer>
      <!-- <div v-if="$auth.loggedIn == true">
        <v-btn small outlined depressed fab color="teal" class="me-3">
          <v-icon>mdi-note-plus-outline</v-icon>
        </v-btn>
        <v-btn small outlined depressed fab color="teal" class="me-3">
          <v-icon>mdi-tag</v-icon>
        </v-btn>
      </div> -->
      <v-toolbar-title>Wall of Sharing</v-toolbar-title>
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
      <!-- <v-main> -->
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
    async clickSearch() {
     this.$nuxt.$emit('click-search',  this.search);
    },

    async getPostWithTags(itemValue) {
      this.$nuxt.$emit("click-tags", itemValue);
    },
  },
};
</script>
