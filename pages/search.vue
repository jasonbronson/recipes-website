<template>
  <div>
    <Header @search="performSearch" />
    <main>
      <section v-if="results.length > 0" class="search-results">
        <h2>Search Results</h2>
        <div class="container">
          <div class="card" v-for="recipe in results" :key="recipe.title">
            <a :href="`/recipes/${recipe.category}/${recipe.title}`">
              <img
                :src="recipe.image"
                :alt="recipe.title"
                class="recipe-image"
              />
            </a>
            <div class="category">
              {{ capitalizeFirstLetter(recipe.category) }}
            </div>
            <a :href="`/recipes/${recipe.category}/${recipe.title}`">
              <h3>{{ recipe.title }}</h3>
            </a>
            <div class="date">{{ formatDate(recipe.date) }}</div>
          </div>
        </div>
      </section>
      <section v-else>
        <h1>No results found...</h1>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRoute } from "vue-router";
import Header from "~/components/Header.vue";
import { config } from "~/config";

const route = useRoute();
const results = ref([]);

const performSearch = async (term) => {
  try {
    const response = await axios.get(
      `${config.apiBaseUrl}/search-recipes?q=${term}`
    );
    results.value = response.data;
  } catch (error) {
    console.error("Error fetching search results:", error);
  }
};

// Fetch search term from query parameters on mount
onMounted(() => {
  const searchTerm = route.query.q || "";
  if (searchTerm) {
    performSearch(searchTerm);
  }
});

// Method to format the date
const formatDate = (dateString) => {
  const options = { year: "numeric", month: "short", day: "2-digit" };
  const date = new Date(dateString);
  return date
    .toLocaleDateString("en-US", options)
    .replace(",", "")
    .replace(" ", " ");
};

// Method to capitalize the first letter of the category
const capitalizeFirstLetter = (string) => {
  if (!string) return "";
  return string.charAt(0).toUpperCase() + string.slice(1);
};
</script>

<style scoped>
.search-results {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.container {
  display: flex;
  flex-wrap: wrap; /* Allow cards to wrap to the next line */
}

.card {
  flex: 1 1 30%; /* Allow cards to take up to 30% of the container width */
  margin: 10px; /* Add some margin between cards */
}

.recipe-image {
  width: 100%; /* Set the width to 100% of the card */
  height: 200px; /* Set a fixed height */
  object-fit: cover; /* Ensure the image covers the area without distortion */
}
</style>
