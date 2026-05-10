<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const { data: categories } = await useFetch('http://localhost:1337/api/categories')

const searchQuery = ref('')
const router = useRouter()

const handleSearch = () => {
  if (searchQuery.value.trim()) {
    router.push(`/search?q=${searchQuery.value.trim()}`)
    searchQuery.value = '' 
  }
}
</script>

<template>
  <div class="layout-wrapper">
    <header class="navbar">
      <div class="nav-content">
        <NuxtLink to="/" class="logo">My Tech Blog</NuxtLink>
        
        <nav class="nav-links">
          <NuxtLink to="/" class="nav-link">Home</NuxtLink>
          <NuxtLink 
            v-for="category in categories?.data" 
            :key="category.id" 
            :to="`/category/${category.Name}`" 
            class="nav-link"
          >
            {{ category.Name }}
          </NuxtLink>
        </nav>

        <div class="search-container">
          <input 
            v-model="searchQuery" 
            @keyup.enter="handleSearch" 
            type="text" 
            placeholder="Search posts..." 
            class="search-input"
          />
        </div>
        
      </div>
    </header>

    <main class="main-content">
      <slot />
    </main>

    <footer class="footer">
      <p>&copy; 2026 My Tech Blog. Built with Nuxt & Strapi.</p>
    </footer>
  </div>
</template>

<style scoped>
.layout-wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.navbar {
  background-color: #2c3e50;
  color: white;
  padding: 15px 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.nav-content {
  max-width: 800px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  color: white;
  font-size: 1.5rem;
  font-weight: bold;
  text-decoration: none;
}

.nav-links {
  display: flex;
  gap: 20px; 
}

.nav-link {
  color: #ecf0f1;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s;
}

.nav-link:hover {
  color: #3498db;
}

.router-link-active {
  color: #3498db;
  border-bottom: 2px solid #3498db;
  padding-bottom: 4px;
}

.logo.router-link-active {
  border-bottom: none;
  color: white;
}

.main-content {
  flex: 1; 
  background-color: #f9f9f9;
}

.footer {
  background-color: #2c3e50;
  color: #bdc3c7;
  text-align: center;
  padding: 20px;
  font-size: 0.9rem;
}
.search-container {
  display: flex;
  align-items: center;
}

.search-input {
  padding: 8px 12px;
  border-radius: 20px;
  border: none;
  outline: none;
  font-family: inherit;
  font-size: 0.9rem;
  width: 200px;
  transition: box-shadow 0.2s;
}

.search-input:focus {
  box-shadow: 0 0 0 2px #3498db;
}
</style>