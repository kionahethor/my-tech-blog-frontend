<script setup>
import { ref } from 'vue'
const page = ref(1)

const pageSize = 2
const { data: posts } = await useFetch(() =>
    `http://localhost:1337/api/posts?populate=*&pagination[page]=${page.value}&pagination[pageSize]=${pageSize}`
)

const nextPage = () => {
    if (page.value < posts.value.meta.pagination.pageCount) {
        page.value++
        window.scrollTo({ top: 0, behavior: 'smooth' })
    }
}

const prevPage = () => {
    if (page.value > 1) {
        page.value--
        window.scrollTo({ top: 0, behavior: 'smooth' })
    }
}
</script>
<template>
    <div class="blog-container">
        <h1 class="page-title">My Blog Posts</h1>

        <div v-for="post in posts?.data" :key="post.id" class="post-card">
            <img v-if="post.CoverImage" :src="`http://localhost:1337${post.CoverImage.url}`" alt="Blog cover image"
                class="cover-image" />

            <div class="post-content">
                <NuxtLink :to="`/${post.documentId}`" class="post-link">
                    <h2>{{ post.Title }}</h2>
                </NuxtLink>
                <p class="post-snippet">{{ post.Snippet }}</p>
            </div>
        </div>

        <div class="pagination-controls" v-if="posts?.meta?.pagination">
            <button @click="prevPage" :disabled="page === 1" class="page-btn">
                ← Previous
            </button>

            <span class="page-info">
                Page {{ posts.meta.pagination.page }} of {{ posts.meta.pagination.pageCount }}
            </span>

            <button @click="nextPage" :disabled="page === posts.meta.pagination.pageCount" class="page-btn">
                Next →
            </button>
        </div>

    </div>
</template>

<style scoped>
.blog-container {
    max-width: 800px;
    margin: 40px auto;
    padding: 0 20px;
}

.page-title {
    text-align: center;
    margin-bottom: 40px;
    font-size: 2.5rem;
}

.post-card {
    background: #ffffff;
    border: 1px solid #eaeaea;
    border-radius: 10px;
    margin-bottom: 24px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
    transition: transform 0.2s, box-shadow 0.2s;
    overflow: hidden;
}

.post-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
}

.cover-image {
    width: 100%;
    height: 250px;
    object-fit: cover;
    border-bottom: 1px solid #eaeaea;
}

.post-content {
    padding: 24px;
}

.post-link {
    text-decoration: none;
    color: #2c3e50;
}

.post-link h2 {
    margin-top: 0;
    transition: color 0.2s;
}

.post-link:hover h2 {
    color: #3498db;
}

.post-snippet {
    color: #666;
    line-height: 1.6;
    margin-bottom: 0;
}
.pagination-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 40px;
  padding-top: 20px;
  border-top: 2px solid #eaeaea;
}

.page-btn {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s, opacity 0.2s;
}

.page-btn:hover:not(:disabled) {
  background-color: #2980b9;
}

.page-btn:disabled {
  background-color: #bdc3c7;
  cursor: not-allowed;
  opacity: 0.7;
}

.page-info {
  font-size: 1.1rem;
  font-weight: 500;
  color: #7f8c8d;
}
</style>