<template>
  <div>
    <div class="row mt-5">
      <div class="container">
        <div class="col-md-12">
          <b>id</b>
          <p>{{ posts.id }}</p>
          <b>Имя</b>
          <p>{{ posts.name }}</p>
          <b>Email</b>
          <p>{{ posts.email }}</p>
          <b>Текст</b>
          <p>{{ posts.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
//import axios from 'axios'
export default {
  transition: {
    name: "home",
    mode: "out-in",
  },
  data() {
    return {
      id: null,
      posts: [],
    };
  },

  mounted() {
    this.id = this.$route.params.id;
  },

  async fetch() {
    this.posts = await fetch(
      "https://jsonplaceholder.typicode.com/comments"
    ).then((res) => res.json());
    this.posts = this.posts.find((id) => id.id == this.$route.params.id);
  },
};
</script>