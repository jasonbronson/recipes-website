<template>
  <div>
    <Header />

    <main>
      <section v-if="hasRecipes" class="featured-recipes">
        <div class="container">
          <div class="card" v-for="recipe in recipes" :key="recipe.title">
            <a :href="`${recipe.link}`">
              <!-- <img
                :src="recipe.image"
                :alt="recipe.title"
                class="recipe-image"
              /> -->
            </a>
            <div class="category">
              {{ capitalizeFirstLetter(recipe.category) }}
            </div>
            <a :href="`${recipe.link}`">
              <h3>{{ recipe.title }}</h3>
            </a>
            <div class="date">{{ formatDate(recipe.date) }}</div>
          </div>
        </div>
      </section>
      <section v-else>
        <h1>No recipes found...</h1>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, computed } from "vue";
import { useRoute } from "vue-router";
import axios from "axios";
import Header from "~/components/Header.vue";
import { config } from "~/config";

const recipes = ref([]);
const route = useRoute();
const hasRecipes = computed(() => recipes.value && recipes.value.length > 0);

const fetchRecipes = async (category) => {
  try {
    const response = await axios.get(
      `${config.apiBaseUrl}/get-recipes?category=${category}`
    );
    recipes.value = response.data;
  } catch (error) {
    console.error("Error fetching recipes:", error);
  }
};

// Fetch recipes on initial load
onMounted(() => {
  const category = route.query.category || ""; // Get category from query parameters
  fetchRecipes(category); // Fetch recipes based on the category
});

// Watch for changes in the category query parameter
watch(
  () => route.query.category,
  (newCategory) => {
    fetchRecipes(newCategory || ""); // Fetch recipes whenever the category changes
  }
);

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
/* Add your homepage styles here */
.recipe-image {
  width: 100%; /* Set the width to 100% of the card */
  height: 200px; /* Set a fixed height */
  object-fit: cover; /* Ensure the image covers the area without distortion */
}
.container {
  display: flex;
  flex-wrap: wrap; /* Allow cards to wrap to the next line */
}
.card {
  flex: 1 1 30%; /* Allow cards to take up to 30% of the container width */
  margin: 10px; /* Add some margin between cards */
}
</style>
