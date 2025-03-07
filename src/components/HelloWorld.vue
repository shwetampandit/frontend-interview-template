<template>
  <div class="hello">
    <h1>Posts</h1>
    <input v-model="searchInput" placeholder="Search posts..." class="search-input" />

    <li v-for="post in paginatedPosts" :key="post.id">
      <div class="card">
        <p><strong>{{ post.title }}</strong></p>
        <p>{{ post.body }}</p>
        <p>By : {{ post.userName }}</p>
      </div>
    </li>
    <Pagination :currentPage="currentPage" :totalPages="totalPages" @update:page="setPage" />
  </div>
</template>

<script setup>
/* prettier-ignore */
import { onMounted, computed, ref } from 'vue';
import Pagination from "./Pagination.vue";

const users = ref([]);
const posts = ref([]);
const searchInput = ref("");
const currentPage = ref(1);
const postsPerPage = 10;

const fetchData = async () => {
  try {
    const [userData, postsData] = await Promise.all([
      fetch("https://jsonplaceholder.typicode.com/users").then((res) =>
        res.json()
      ),
      fetch("https://jsonplaceholder.typicode.com/posts").then((res) =>
        res.json()
      ),
    ]);
    users.value = userData;
    posts.value = postsData.map((post) => ({
      ...post,
      userName: userData.find((user) => user.id === post.userId)?.username || "Unknown",
    }));
  } catch (error) {
    console.lof(error)
  }
}
onMounted(fetchData);

const mappedData = computed(() =>
  posts.value.filter(
    (post) =>
      post.title.toLowerCase().includes(searchInput.value.toLowerCase()) ||
      post.body.toLowerCase().includes(searchInput.value.toLowerCase())
  )
);

const totalPages = computed(() => Math.ceil(mappedData.value.length / postsPerPage));

const paginatedPosts = computed(() => {
  const start = (currentPage.value - 1) * postsPerPage;
  return mappedData.value.slice(start, start + postsPerPage);
});

const setPage = (page) => {
  currentPage.value = page;
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.card {
  padding: 0 1rem;
  border: 2px solid grey;
  border-radius: 24px;
  margin: 1rem;
  width: 50rem;
}

.search-input {
  width: 50rem;
  padding: 8px;
  margin-bottom: 10px;
  border: 2px solid grey;
  border-radius: 12px;
}
</style>
