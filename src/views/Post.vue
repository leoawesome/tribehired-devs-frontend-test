<template lang="pug">
.Post
  p Post:
  .post(v-if="post !== ''")
    p {{post.id}}. #[b {{post.title}}]
    p {{post.body}}
    p By: User No.{{post.userId}}
  p Search box:
  input(type="text" v-model="filterString")
  p Comments:
  .comments(v-if="comments.length > 0")
    .comment(v-for="comment in comments")
      p {{comment.body}}
      p By: {{comment.name}} <{{comment.email}}>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Post',
  data() {
    return {
      post: '',
      filterString: '',
      initComments: [],
    };
  },
  computed: {
    comments() {
      return this.filterString === ''
        ? this.initComments
        : this.initComments
          .filter((comment) => (
            Object.keys(comment).some((field) => String(comment[field]).includes(this.filterString))
          ));
    },
  },
  async created() {
    const postRes = await axios.get(`https://jsonplaceholder.typicode.com/posts/${this.$route.params.id}`);
    this.post = postRes.data;

    const commentRes = await axios.get(`https://jsonplaceholder.typicode.com/comments?postId=${this.$route.params.id}`);
    this.initComments = commentRes.data;
  },
};
</script>

<style lang="sass" scoped>
.post
  border: 5px solid red
  margin: 20px
  padding: 10px

.comments
  border: 5px solid black
  margin: 20px

  .comment
    border: 1px solid black
    padding: 10px
</style>
