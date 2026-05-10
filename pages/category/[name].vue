<script setup>
const route = useRoute()
const categoryName = route.params.name
const { data: posts } = await useFetch(`http://localhost:1337/api/posts?filters[category][Name][$eq]=${categoryName}&populate=*`)
</script>

<template>
  <div class="blog-container">
    <h1 class="page-title">Category: {{ categoryName }}</h1>
    
    <div v-if="posts && posts.data && posts.data.length > 0">
      <div v-for="post in posts.data" :key="post.id" class="post-card">
        <NuxtLink :to="`/${post.documentId}`" class="post-link">
          <h2>{{ post.Title }}</h2>
        </NuxtLink>
        <p class="post-snippet">{{ post.Snippet }}</p>
      </div>
    </div>
    
    <div v-else class="empty-state">
      <p>No posts found in this category.</p>
    </div>

    <NuxtLink to="/" class="back-btn mt-4">← Back to Home</NuxtLink>
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
  color: #2c3e50;
}

.post-card {
  background: #ffffff;
  border: 1px solid #eaeaea;
  border-radius: 10px;
  padding: 24px;
  margin-bottom: 24px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  transition: transform 0.2s, box-shadow 0.2s;
}

.post-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.1);
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