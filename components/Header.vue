<template>
  <header>
    <div class="logo">
      <router-link to="/">
        <img src="/logo.png" alt="Bronson Cooking Site" />
      </router-link>
    </div>
    <nav>
      <ul>
        <li>
          <a href="#" @click.prevent="navigateToCategory('baking')">Baking</a>
        </li>
        <li>
          <a href="#" @click.prevent="navigateToCategory('dinner')">Dinner</a>
        </li>
        <li>
          <a href="#" @click.prevent="navigateToCategory('breakfast')"
            >Breakfast</a
          >
        </li>
      </ul>
    </nav>
    <div class="search">
      <input type="text" placeholder="Search recipe..." />
      <button type="submit">🔍</button>
    </div>
  </header>
</template>

<script setup>
import { useRouter } from "vue-router";

const router = useRouter();

const navigateToCategory = (category) => {
  router.push({ path: "/", query: { category } });
};
</script>

<style scoped>
/* Add your styles here if needed */
</style>
