<template>
  <v-sheet elevation="5">
    <v-chip-group
      multiple
      active-class="primary white--text"
      show-arrows
      class="me-2 ms-2"
      v-model="itemValue"
    >
      <v-chip v-for="tag in tags" :key="tag.id">
        {{ tag.name }}
      </v-chip>
    </v-chip-group>
  </v-sheet>
</template>

<script>
export default {
  data() {
    return {
      tags: [],
      itemValue: [],
    };
  },

  watch: {
    itemValue: {
      handler() {
         this.$emit("post-with-tags", this.itemValue);
      }
    }
  },

  async fetch() {
    const { data } = await this.$axios.get("/tags?limit=*");
    this.tags = data.data;
  },
};
</script>

<style lang="scss" scoped></style>
