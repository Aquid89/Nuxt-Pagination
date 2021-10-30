<template>
  <div>
    <h2 class="text-center my-5">Cраница: {{ id }} из {{ pageCount }}</h2>
    <button @click="sortById()" class="btn btn-link">Сортировать</button>
    <transition name="home" mode="out-in">
      <table class="table">
        <tbody>
          <tr v-for="post in paginatedData" :key="post.id">
            <td>{{ post.id }}</td>
            <td>{{ post.name }}</td>
            <td>{{ post.email }}</td>
            <td>
              <NuxtLink :to="`/article/${post.id}`" class="nav-link"
                >Читать далее...</NuxtLink
              >
            </td>
          </tr>
        </tbody>
      </table>
    </transition>
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item">
          <NuxtLink
            :to="`/page/${this.prev}`"
            class="page-link"
            v-show="this.prev != 0"
          >
            <span aria-hidden="true">&laquo;</span>
          </NuxtLink>
        </li>
        <template v-for="n in pageCount">
          <li class="page-item" :key="n">
            <NuxtLink :to="`/page/${n}`" class="nav-link">{{ n }}</NuxtLink>
          </li>
        </template>
        <li class="page-item">
          <NuxtLink
            :to="`/page/${this.next}`"
            class="page-link"
            v-show="this.next <= pageCount"
          >
            <span aria-hidden="true">&raquo;</span>
          </NuxtLink>
        </li>
      </ul>
    </nav>
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
      next: null,
      prev: null,
      paginatedData: [],
      isActive: true,
    };
  },
  mounted() {
    this.id = this.$route.params.id;
    this.next = this.$route.params.id;
    this.prev = this.$route.params.id;
    this.next++;
    this.prev--;
  },
  methods: {
    sortById() {
      var t = this.paginatedData;
      var byId = t.slice(0);
      if (this.isActive) {
        byId.sort(function (a, b) {
          return b.id - a.id;
        });
      } else {
        byId.sort(function (a, b) {
          return a.id - b.id;
        });
      }

      this.isActive = !this.isActive;
      this.paginatedData = byId;
    },
  },
  computed: {
    pageCount() {
      let l = this.posts.length,
        s = 10;
      return Math.ceil(l / s);
    },
  },
  async fetch() {
    this.posts = await fetch(
      "https://jsonplaceholder.typicode.com/comments"
    ).then((res) => res.json());
    const start = (this.$route.params.id - 1) * 10,
      end = start + 10;
    this.paginatedData = this.posts.slice(start, end);
  },
};
</script>
<style>
.home-enter-active,
.home-leave-active {
  transition: opacity 0.5s;
}
.home-enter,
.home-leave-active {
  opacity: 0;
}
.pagination {
  max-width: 600px;
  flex-wrap: wrap;
  margin: auto;
}
.nuxt-link-active {
  font-weight: bold;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.page-link {
  cursor: pointer;
}
</style>