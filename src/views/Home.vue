<template lang="pug">
.Home
  p Posts:
  .post(v-for="post in posts" @click="navigate(post.id)")
    p {{post.id}}. #[b {{post.title}}]
    p {{post.body}}
    p By: User No.{{post.userId}}
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',
  data() {
    return {
      posts: [],
    };
  },
  async created() {
    const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
    this.posts = res.data;
  },
  methods: {
    navigate(postID) {
      this.$router.push(`/post/${postID}`);
    },
  },
};
</script>

<style lang="sass">
.post
  border: 5px solid red;
  margin: 20px;
  padding: 10px;
</style>
