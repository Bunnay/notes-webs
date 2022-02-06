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
      </v-row>
      <v-spacer></v-spacer>
      <v-toolbar-title>Wall of Sharing</v-toolbar-title>
    </v-app-bar>
      <chip-group :style="{marginTop: '64px', position: 'fixed', zIndex: '1', width: '100%'}"></chip-group>
      <v-main :style="{marginTop: '60px'}">
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
      tags: [
        "Work",
        "Home Improvement",
        "Vacation",
        "Food",
        "Drawers",
        "Shopping",
        "Art",
        "Tech",
        "Creative Writing",
        "Work",
        "Home Improvement",
        "Vacation",
        "Food",
        "Drawers",
        "Shopping",
        "Art",
        "Tech",
        "Creative Writing",
      ],
    };
  },
};
</script>
