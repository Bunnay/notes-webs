<template>
  <div class="d-flex flex-wrap">
    <v-card
      class="box-shadow me-2 ms-2 mt-2 mb-2"
      width="330"
      v-for="post in posts"
      :key="post.id"
    >
      <v-img :src="image" v-if="post.image"></v-img>
      <v-card-title>
        {{ post.title }}
      </v-card-title>
      <v-card-subtitle>
        {{ post.body }}
      </v-card-subtitle>
      <v-card-text>
        <div class="d-flex">
          <a
            v-for="(tag, index) in post.tags"
            :key="index"
            class="me-2 primary--text"
            @click="clickTags(tag.id)"
          >
            {{ "#" + tag.name }}
          </a>
        </div>
      </v-card-text>

      <v-card-actions>
        <v-btn depressed class="me-1">
          <v-icon small class="me-2">mdi-thumb-up-outline</v-icon>
          Like
        </v-btn>
        <v-btn depressed>
          <v-icon small class="me-2">mdi-thumb-down-outline</v-icon>
          Dislike
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
    };
  },

  methods: {
    async clickTags(id) {
      const {data} = await this.$axios.get('/posts?filter[tags.id]=' + id + '&include=tags')
      this.posts = data.data;
      console.log(this.posts);
    }
  },

  async fetch() {
    const { data } = await this.$axios.get("/posts?include=tags");
    this.posts = data.data;
  },
};
</script>

<style lang="scss" scoped></style>
