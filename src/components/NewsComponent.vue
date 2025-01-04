<template>
  <div>
    <h1>Latest News</h1>
    <div v-if="loading">Loading...</div>
    <ul v-else>
      <li v-for="(article, index) in articles" :key="index">
        <a :href="article.url" target="_blank">{{ article.title }}</a>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      articles: [],
      loading: true,
    };
  },
  created() {
    // Замість YOUR_API_KEY вставте свій ключ API
    const apiKey = '7d5231235c414365865d67953db24b86';
    const url = `https://newsapi.org/v2/top-headlines?country=us&apiKey=${apiKey}`;

    axios.get(url)
      .then(response => {
        this.articles = response.data.articles;
        this.loading = false;
      })
      .catch(error => {
        console.log(error);
        this.loading = false;
      });
  },
};
</script>

<style scoped>
h1 {
  font-size: 2em;
  text-align: center;
  margin-top: 20px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 10px;
}

a {
  text-decoration: none;
  color: blue;
}
</style>
