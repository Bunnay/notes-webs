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
        onMouseOver="this.style.backgroundColor='#272727'"
        onMouseOut="this.style.backgroundColor='#1E1E1E'"
        width="338"
        v-for="(post, index) in posts"
        :key="index"
        outlined
      >
        <v-img :src="post.image" v-if="post.image"></v-img>
        <v-card-title>
          {{ post.title }}
        </v-card-title>
        <v-card-subtitle>
          {{ post.body }}
        </v-card-subtitle>
        <v-card-text>
          <div class="d-flex">
            <!-- <a
              v-for="(tag, index) in post.tags"
              :key="index"
              class="me-2 primary--text"
              @click="clickTags(tag.id)"
            >
              {{ "#" + tag.name }}
            </a> -->
            <div
              v-for="(tag, index) in post.tags"
              :key="index"
              class="me-2 primary--text"
            >
              {{ "#" + tag.name }}
            </div>
          </div>
        </v-card-text>
        <v-spacer></v-spacer>
        <v-card-actions class="mb-2">
          <v-badge
            color="green"
            bordered
            class="me-5"
            overlap
            :value="post.likes > 0"
          >
            <template v-slot:badge>
              {{ post.likes }}
            </template>
            <v-btn
              depressed
              class="me-1"
              @click="clickLike(post.id, index)"
              :disabled="loading"
            >
              <v-icon small class="me-2">mdi-thumb-up-outline</v-icon>
              Like
            </v-btn>
          </v-badge>
          <v-badge color="red" bordered overlap :value="post.dislikes > 0">
            <template v-slot:badge>
              {{ post.dislikes }}
            </template>
            <v-btn
              depressed
              @click="clickDisLike(post.id, index)"
              :disabled="loading"
            >
              <v-icon small class="me-2">mdi-thumb-down-outline</v-icon>
              Dislike
            </v-btn>
          </v-badge>
        </v-card-actions>
      </v-card>
    </div>
    <div v-if="posts.length == 0 && !loading" class="text-center mt-3">
      <h4>No posts found. :(</h4>
    </div>

    <div
      class="d-flex justify-content-center my-5"
      v-if="links && links.length > 3"
    >
      <nav aria-label="Page navigation example">
        <ul class="pagination">
          <li
            v-for="(link, index) in links"
            :key="index"
            :class="link.active ? 'page-item active' : 'page-item'"
          >
            <button
              :class="link.active ? 'page-link' : 'page-link bg-dark border-0'"
              v-on:click="onPaginationLinkClick(link.url)"
              :disabled="!link.url"
            >
              {{
                link.label.includes("Next")
                  ? "Next"
                  : link.label.includes("Previous")
                  ? "Previous"
                  : link.label
              }}
            </button>
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
      links: [],
      loading: false,
      disableButton: false,
    };
  },

  methods: {
    async onPaginationLinkClick(url) {
      if (!url) return;

      this.loading = true;
      const { data } = await this.$axios.get(url);
      this.posts = data.data;
      this.links = data.meta?.links ?? [];
      this.loading = false;
    },
    async clickTags(id) {
      const { data } = await this.$axios.get(
        "/posts?filter[tags.id]=" + id + "&include=tags&limit=18"
      );
      this.posts = data.data;
      this.links = data.meta?.links ?? [];
    },

    async clickLike(id, index) {
      this.loading = true;
      await this.$axios.get("/posts/" + id + "/like");
      this.posts[index].likes++;
      this.loading = false;
    },

    async clickDisLike(id, index) {
      this.loading = true;
      await this.$axios.get("/posts/" + id + "/dislike");
      this.posts[index].dislikes++;
      this.loading = false;
    },

    async fetchPosts() {
      this.$nuxt.$emit("fetch-posts", this.posts);
    },

    async clickSearch(search) {
      this.loading = true;
      const { data } = await this.$axios.get(
        `/posts?sort=-created_at&include=tags&limit=${
          search ? "*" : "18"
        }&filter[title]=${search}`
      );
      this.posts = data.data;
      this.links = data.meta?.links ?? [];
      this.loading = false;
    },
    async clickBarTags(itemValue) {
      this.loading = true;
      const res = [];
      for (let i = 0; i < itemValue.length; i++) {
        res[i] = itemValue[i];
      }
      for (let i = 0; i < res.length; i++) {
        res[i] += 1;
      }
      const { data } = await this.$axios.get(
        "/posts?sort=-created_at&filter[tags.id]=" +
          res.toString() +
          "&include=tags&limit=18"
      );
      this.posts = data.data;
      this.links = data.meta?.links ?? [];
      this.loading = false;
    },
  },

  created() {
    this.$nuxt.$on("click-tags", (itemValue) => {
      this.clickBarTags(itemValue);
    });

    this.$nuxt.$on("click-search", (search) => {
      this.clickSearch(search);
    });

    this.$nuxt.$on("fetch-posts", (posts) => {
      this.$fetch();
    });
  },

  async fetch() {
    this.loading = true;
    const { data } = await this.$axios.get(
      "/posts?sort=-created_at&include=tags&limit=18"
    );
    this.posts = data.data;
    this.links = data.meta?.links ?? [];
    this.loading = false;
  },
};
</script>

<style lang="scss" scoped></style>
