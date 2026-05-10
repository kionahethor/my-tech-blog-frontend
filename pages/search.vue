<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const searchQuery = computed(() => route.query.q)
const { data: posts } = await useFetch(() => `http://localhost:1337/api/posts?filters[Title][$containsi]=${searchQuery.value}&populate=*`)
</script>

<template>
  <div class="blog-container">
    <h1 class="page-title">Search Results for: "<span class="highlight">{{ searchQuery }}</span>"</h1>
    
    <div v-if="posts && posts.data && posts.data.length > 0">
      <div v-for="post in posts.data" :key="post.id" class="post-card">
        
        <img 
          v-if="post.CoverImage" 
          :src="`http://localhost:1337${post.CoverImage.url}`" 
          alt="Blog cover image" 
          class="cover-image"
        />

        <div class="post-content">
          <NuxtLink :to="`/${post.documentId}`" class="post-link">
            <h2>{{ post.Title }}</h2>
          </NuxtLink>
          <p class="post-snippet">{{ post.Snippet }}</p>
        </div>
      </div>
    </div>
    
    <div v-else class="empty-state">
      <p>Bummer! We couldn't find any posts matching "{{ searchQuery }}".</p>
      <NuxtLink to="/" class="back-btn">← Back to all posts</NuxtLink>
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
  font-size: 2.2rem;
  color: #2c3e50;
}

.highlight {
  color: #3498db;
}

.post-card {
  background: #ffffff;
  border: 1px solid #eaeaea;
  border-radius: 10px;
  margin-bottom: 24px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  transition: transform 0.2s, box-shadow 0.2s;
  overflow: hidden;
}

.post-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.1);
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

.post-link:hover h2 {
  color: #3498db;
}

.post-snippet {
  color: #666;
  line-height: 1.6;
}

.empty-state {
  text-align: center;
  font-size: 1.2rem;
  color: #7f8c8d;
  padding: 40px;
}

.back-btn {
  display: inline-block;
  text-decoration: none;
  color: #3498db;
  font-weight: 600;
  margin-top: 20px;
}

.back-btn:hover {
  text-decoration: underline;
}
</style>