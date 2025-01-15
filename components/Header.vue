<template>
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
      <input type="text" placeholder="Search recipe..." />
      <button type="submit">Search</button>
    </div>
  </header>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const isMenuOpen = ref(false);

const navigateToCategory = (category) => {
  router.push({ path: "/", query: { category } });
  isMenuOpen.value = false; // Close menu after navigation
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
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
    right: 0; /* Aligns to the right edge */
    background-color: #fff;
    flex-direction: column;
    width: 200px; /* Adjust width as needed */
  }

  nav.open {
    display: flex;
  }
}

nav {
  display: flex;
}

nav ul {
  list-style: none;
  display: flex;
  gap: 1rem;
  margin: 0;
  padding: 0;
}

nav ul li a {
  color: #fff;
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

@media (max-width: 768px) {
  .menu-toggle {
    display: block;
  }

  nav {
    display: none;
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
    display: flex;
  }

  .search {
    order: 2;
    margin-top: 1rem;
    width: 100%;
  }
}
</style>
