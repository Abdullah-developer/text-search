<template>
  <div class="article-search">
    <h1>Search</h1>
    <input
      v-model="searchQuery"
      @input="onSearchChange"
      placeholder="Search articles..."
      class="search-input" />

    <p v-if="!filteredArticles.length" class="no-results">No articles found.</p>
    <p v-else class="results-count">
      {{ filteredArticles.length }} posts were found.
    </p>

    <ul class="article-list">
      <li
        v-for="article in filteredArticles"
        :key="article.isbn"
        class="article-item">
        <a
          :href="article.url"
          target="_blank"
          class="article-title"
          v-html="highlightMatches(article.title)"></a>
        <p class="article-meta">{{ article.author }} - {{ article.year }}</p>
        <p class="article-summary">{{ article.summary }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import articles from "./assets/articles.json";
export default {
  data() {
    return {
      searchQuery: "",
      articles,
    };
  },
  computed: {
    filteredArticles() {
      const query = this.searchQuery.toLowerCase();
      if (!query) return this.articles;

      return this.articles.filter((article) =>
        article.title.toLowerCase().includes(query)
      );
    },
  },
  methods: {
    highlightMatches(title) {
      const query = this.searchQuery.toLowerCase();
      if (!query) return title;

      const regex = new RegExp(`(${query})`, "gi");
      return title.replace(
        regex,
        (match) => `<span class="highlight">${match}</span>`
      );
    },
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  line-height: 1.6;
  background-color: #f9f9f9;
  margin: 0;
  padding: 0;
}

.article-search {
  max-width: 800px;
  margin: 30px auto;
  padding: 20px;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  margin-bottom: 20px;
  font-size: 24px;
  color: #333333;
}

.search-input {
  width: 100%;
  padding: 10px 15px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 20px;
  box-sizing: border-box;
}

.results-count {
  margin-bottom: 10px;
  font-weight: bold;
  color: #555555;
}

.no-results {
  color: #ff0000;
  text-align: center;
  margin: 20px 0;
}

.article-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.article-item {
  padding: 15px;
  border-bottom: 1px solid #eee;
}

.article-title {
  font-size: 18px;
  font-weight: bold;
  color: #666;
  text-decoration: none;
  transition: color 0.15s ease-in-out;
}

.article-title:hover {
  color: #999;
}

.article-meta {
  color: #666666;
  font-size: 14px;
  margin: 5px 0;
}

.article-summary {
  color: #444444;
  font-size: 16px;
}

.highlight {
  background-color: yellow;
  font-weight: bold;
}
</style>
