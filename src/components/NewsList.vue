<template>
  <div class="container">
    <!-- Header with Navigation -->
    <header>
      <nav>
        <div class="logo">My News App</div>
        <ul class="menu">
          <li><a href="#">Home</a></li>
          <li><a href="#">Politics</a></li>
          <li><a href="#">Business</a></li>
          <li><a href="#">Technology</a></li>
          <li><a href="#">Science</a></li>
        </ul>
      </nav>
    </header>

    <!-- Main Content -->
    <main>
      <h2 class="headline">News Articles about "{{ query }}"</h2>

      <!-- Loading Indicator -->
      <div v-if="loading" class="loading">Loading news...</div>

      <!-- Articles -->
      <div v-if="!loading && articles.length">
        <div class="news-grid">
          <div v-for="(article, index) in articles" :key="index" class="news-card">
            <img :src="article.urlToImage" alt="Article image" v-if="article.urlToImage" class="news-image" />
            <div class="news-content">
              <h3 class="news-title">{{ article.title }}</h3>
              <p class="news-description">{{ article.description }}</p>
              <a :href="article.url" target="_blank" class="read-more">Read more</a>
            </div>
          </div>
        </div>
      </div>

      <!-- No Articles Found -->
      <div v-else-if="!loading && !articles.length">
        <p>No articles found for "{{ query }}".</p>
      </div>

      <!-- Error Handling -->
      <div v-if="error" class="error">
        <p>Error fetching news. Please try again later.</p>
      </div>
    </main>

    <footer>
      <p>&copy; 2025 My News App - All rights reserved</p>
    </footer>
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
      articles: [],
      loading: true,
      error: false
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
            this.articles = data.articles;
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
/* General Layout */
.container {
  font-family: Arial, sans-serif;
  margin: 0 auto;
  padding: 0;
}

/* Header with Navigation */
header {
  background-color: #333;
  color: white;
  padding: 15px 20px;
}

nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

nav .logo {
  font-size: 1.8em;
  font-weight: bold;
}

nav .menu {
  list-style-type: none;
  display: flex;
  gap: 20px;
}

nav .menu li {
  display: inline;
}

nav .menu a {
  color: white;
  text-decoration: none;
}

nav .menu a:hover {
  text-decoration: underline;
}

/* Main Content */
main {
  padding: 20px;
  background-color: #f4f4f4;
}

.headline {
  text-align: center;
  font-size: 2.5em;
  margin-bottom: 30px;
}

.news-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}

.news-card {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.news-card .news-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.news-content {
  padding: 15px;
}

.news-title {
  font-size: 1.5em;
  margin-bottom: 15px;
}

.news-description {
  font-size: 1em;
  margin-bottom: 10px;
  color: #555;
}

.read-more {
  color: #0073e6;
  font-weight: bold;
}

.read-more:hover {
  text-decoration: underline;
}

/* Error and Loading */
.loading {
  font-size: 1.2em;
  text-align: center;
}

.error {
  color: red;
  text-align: center;
}

/* Footer */
footer {
  text-align: center;
  background-color: #333;
  color: white;
  padding: 20px;
  margin-top: 30px;
}
</style>
