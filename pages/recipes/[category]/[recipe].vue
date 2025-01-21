<template>
  <div>
    <Header />
    <main v-if="recipe">
      <article class="recipe">
        <header class="recipe-header">
          <div class="title-section">
            <h1>{{ recipe.title }}</h1>
            <div class="image-section">
              <img :src="recipe.image" :alt="recipe.title" />
            </div>
            <div class="meta">
              <div class="meta-item">
                <span class="label">Prep Time:</span>
                <span class="value">{{ recipe.prepTime }} mins</span>
              </div>
              <div class="meta-item">
                <span class="label">Cook Time:</span>
                <span class="value">{{ recipe.cookTime }} mins</span>
              </div>
              <div class="meta-item">
                <span class="label">Total Time:</span>
                <span class="value">{{ recipe.totalTime }} mins</span>
              </div>
              <div class="meta-item">
                <span class="label">Servings:</span>
                <span class="value">{{ recipe.servings }}</span>
              </div>
            </div>
            <div class="original-url">
              <span class="label">Original Recipe:</span>
              <a :href="recipe.originalURL" target="_blank">{{
                recipe.originalURL
              }}</a>
            </div>
          </div>
        </header>

        <section class="ingredients">
          <h2>Ingredients</h2>
          <ul>
            <li v-for="ingredient in recipe.ingredients" :key="ingredient">
              {{ ingredient }}
            </li>
          </ul>
        </section>

        <section class="instructions">
          <h2>Instructions</h2>
          <ol>
            <li v-for="instruction in recipe.instructions" :key="instruction">
              {{ instruction }}
            </li>
          </ol>
        </section>
      </article>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import axios from "axios";
import { useRoute } from "vue-router";
import Header from "~/components/Header.vue";
import { config } from "~/config";

const route = useRoute();
const recipe = ref(null);

onMounted(async () => {
  try {
    const recipeName = route.params.recipe;
    const response = await axios.get(
      `${config.apiBaseUrl}/get-recipe/${recipeName}`
    );
    recipe.value = response.data;
  } catch (error) {
    console.error("Error fetching recipe:", error);
  }
});
</script>

<style scoped>
.recipe {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.recipe-header {
  display: grid;
  grid-template-columns: 1fr 400px;
  gap: 2rem;
  margin-bottom: 2rem;
}

@media (max-width: 768px) {
  .recipe-header {
    grid-template-columns: 1fr;
  }

  .image-section {
    margin-top: 1rem;
  }

  .image-section img {
    width: 100%;
    height: auto;
  }
}

.title-section {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.title-section h1 {
  margin: 0;
  font-size: 2.5rem;
  color: #333;
}

@media (max-width: 768px) {
  .title-section h1 {
    font-size: 1.5rem;
  }
}

.meta {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1rem;
  margin: 1rem 0;
}

.meta-item {
  display: flex;
  flex-direction: column;
  padding: 0.5rem;
  background-color: #f5f5f5;
  border-radius: 8px;
}

.label {
  font-weight: bold;
  color: #666;
  font-size: 0.9rem;
}

.value {
  color: #333;
  font-size: 1.1rem;
}

.original-url {
  margin-top: 1rem;
}

.original-url a {
  color: #0066cc;
  text-decoration: none;
  word-break: break-all;
}

.original-url a:hover {
  text-decoration: underline;
}

.image-section img {
  width: 100%;
  height: auto;
  border-radius: 8px;
  object-fit: cover;
}

.ingredients ul,
.instructions ol {
  margin: 1rem 0;
  padding-left: 2rem;
}
</style>
