<template>
  <div>
    <Header />

    <main>
      <section class="hero">
        <h1>Welcome to Bronson Cooking Site</h1>
        <p>Discover our favorite recipes and baking awesomeness.</p>
      </section>

      <section v-if="hasRecipes" class="featured-recipes">
        <h2>Most Recent Recipes</h2>
        <div class="container">
          <div class="card" v-for="recipe in recipes" :key="recipe.title">
            <a :href="`${recipe.link}`">
              <img :src="recipe.image" :alt="recipe.title" />
            </a>
            <div class="category">{{ recipe.category }}</div>
            <a :href="`${recipe.link}`">
              <h3>{{ recipe.title }}</h3>
            </a>
            <div class="date">{{ recipe.date }}</div>
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
import { ref, onMounted, watch } from "vue";
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
</script>

<style scoped>
/* Add your homepage styles here */
</style>
