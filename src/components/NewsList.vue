<template>
  <div>
    <h2>News Articles about "{{ query }}"</h2>

    <!-- Display fetched news articles -->
    <div v-if="loading">Loading news...</div>

    <div v-if="!loading && articles.length">
      <ul>
        <li v-for="(article, index) in articles" :key="index">
             <img :src="article.urlToImage" alt="Article image" v-if="article.urlToImage" />
          <h3>{{ article.title }}</h3>
          <p>{{ article.description }}</p>
          <a :href="article.url" target="_blank">Read more</a>
        </li>
      </ul>
    </div>

    <div v-else-if="!loading && !articles.length">
      <p>No articles found for "{{ query }}".</p>
    </div>

    <div v-if="error" class="error">
      <p>Error fetching news. Please try again later.</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NewsList',
  props: {
    query: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      articles: [],  // To store fetched articles
      loading: true,  // Loading flag
      error: false    // Error flag
    };
  },
  watch: {
    // Watch for changes in the query prop and refetch data
    query(newQuery) {
      this.fetchNews(newQuery);
    }
  },
  mounted() {
    this.fetchNews(this.query);  // Fetch news on initial load
  },
  methods: {
    fetchNews(query) {
      this.loading = true;
      this.error = false;

      // News API URL with dynamic query
const url = `https://newsapi.org/v2/everything?q=${query}&sortBy=popularity&apiKey=${process.env.VUE_APP_API_KEY}`;

      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          if (data.articles) {
            this.articles = data.articles;  // Store the articles in the data property
          } else {
            this.articles = [];
          }
          this.loading = false;
        })
        .catch((error) => {
          console.error('Error fetching news:', error);
          this.error = true;
          this.loading = false;
        });
    }
  }
};
</script>

<style scoped>
.error {
  color: red;
}
</style>
