<template>
  <div class="flex flex-col justify-center mx-auto gap-y-4">
    <PostPreview
      v-for="post in filteredPosts.toReversed()"
      :key="post.title"
      :title="post.title"
      :description="post.description"
      :repo="post.repo"
      :tags="post.tags"
      :thumbnail="post.image"
    />
  </div>
  <div>
    <aside class="hidden lg:flex absolute top-24 right-20">
      <TagLister :posts="posts" />
    </aside>
  </div>
</template>

<script>
import posts from "../assets/posts.json";

import PostPreview from "./PostPreview.vue";
import TagLister from "./TagLister.vue";

export default {
  components: {
    PostPreview,
    TagLister,
  },
  data() {
    return { posts };
  },
  computed: {
    filteredPosts() {
      if (this.$store.state.currentTags.length === 0) {
        return posts;
      } else {
        //could be 2 filter methods, but this feels more readable to me
        const currentPosts = [];
        posts.forEach((post) => {
          if (
            post.tags.filter((tag) =>
              this.$store.state.currentTags.includes(tag)
            ).length > 0
          ) {
            currentPosts.push(post);
          }
        });
        return currentPosts;
      }
    },
  },
};
</script>
