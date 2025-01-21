<template>
  <div>
    <header>
      <div class="logo">
        <router-link to="/">
          <img src="/logo.png" alt="Bronson Cooking Site" />
        </router-link>
      </div>
      <button class="menu-toggle" @click="toggleMenu">☰</button>
      <nav :class="{ open: isMenuOpen }">
        <ul>
          <li>
            <a href="#" @click.prevent="navigateToCategory('')">All</a>
          </li>
          <li>
            <a href="#" @click.prevent="navigateToCategory('baking')">Baking</a>
          </li>
          <li>
            <a href="#" @click.prevent="navigateToCategory('dinner')">Dinner</a>
          </li>
          <li>
            <a href="#" @click.prevent="navigateToCategory('breakfast')">
              Breakfast
            </a>
          </li>
        </ul>
      </nav>
      <div class="search">
        <input
          type="text"
          v-model="searchTerm"
          placeholder="Search recipe..."
        />
        <button @click="performSearch">Search</button>
      </div>
    </header>
    <h3 v-if="isAllCategory">Featured Recipes</h3>
    <section v-if="isAllCategory" class="hero">
      <h1>Welcome to Bronson Cooking Site</h1>
      <p>Discover our favorite recipes and baking awesomeness.</p>
    </section>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { useRouter, useRoute } from "vue-router";

const router = useRouter();
const route = useRoute();
const isMenuOpen = ref(false);
const searchTerm = ref("");

const isAllCategory = computed(() => {
  return !route.query.category || route.query.category === "";
});

const navigateToCategory = (category) => {
  router.push({ path: "/", query: { category } });
  isMenuOpen.value = false; // Close menu after navigation
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

// Perform search and navigate to search results page
const performSearch = () => {
  if (searchTerm.value) {
    router.push({ path: "/search", query: { q: searchTerm.value } });
  }
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background-color: #fff;
  color: #333;
  position: relative;
}

.logo img {
  max-height: 50px;
}

.menu-toggle {
  display: none;
  background: none;
  border: none;
  color: #333;
  font-size: 1.5rem;
  cursor: pointer;
  margin-left: auto;
}

@media (max-width: 768px) {
  .menu-toggle {
    display: block;
  }

  nav {
    display: none; /* Hidden on mobile initially */
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    background-color: #333;
    flex-direction: column;
    gap: 1rem;
    padding: 1rem;
  }

  nav.open {
    display: flex; /* Show the menu when open */
  }

  .search {
    order: 2;
    margin-top: 1rem;
    width: 100%;
  }
}

nav {
  display: flex; /* Ensure the nav is displayed as flex for larger screens */
}

nav ul {
  list-style: none;
  display: flex; /* Ensure the list is displayed as flex */
  gap: 1rem;
  margin: 0;
  padding: 0;
}

nav ul li a {
  color: #333; /* Change color to ensure visibility */
  text-decoration: none;
}

.search {
  display: flex;
  gap: 0.5rem;
}

.search input {
  padding: 0.5rem;
}

.search button {
  padding: 0.5rem;
  background-color: #555;
  color: #fff;
  border: none;
  cursor: pointer;
}
</style>
