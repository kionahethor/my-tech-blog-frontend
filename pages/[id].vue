<script setup>
const route = useRoute()
const { data: post, error } = await useFetch(`http://localhost:1337/api/posts/${route.params.id}?populate=*`)

if (!post.value?.data || error.value) {
    throw createError({
        statusCode: 404,
        statusMessage: 'Post Not Found',
        fatal: true
    })
}
</script>

<template>
    <div class="article-container">
        <div class="article-card">

            <div class="article-header">
                <h1 class="article-title">{{ post.data.Title }}</h1>
                <div class="article-meta">
                    <NuxtLink v-if="post.data.category" :to="`/category/${post.data.category.Name}`"
                        class="badge category-badge">
                        {{ post.data.category.Name }}
                    </NuxtLink>
                    <span v-else class="badge category-badge">No category</span>
                    <span class="author-text">By {{ post.data.author?.Name || 'Unknown author' }}</span>
                </div>
            </div>

            <img v-if="post.data.CoverImage" :src="`http://localhost:1337${post.data.CoverImage.url}`"
                alt="Blog cover image" class="article-cover" />

            <div class="article-content">
        <template v-for="(block, index) in post.data.Content" :key="index">
          
          <component 
            v-if="block.type === 'heading'" 
            :is="`h${block.level}`" 
            class="article-heading"
          >
            <span 
              v-for="(child, childIndex) in block.children" 
              :key="childIndex"
              :class="{ 
                'text-bold': child.bold, 
                'text-italic': child.italic,
                'text-underline': child.underline
              }"
            >
              {{ child.text }}
            </span>
          </component>

          <p v-else-if="block.type === 'paragraph'">
            <span 
              v-for="(child, childIndex) in block.children" 
              :key="childIndex"
              :class="{ 
                'text-bold': child.bold, 
                'text-italic': child.italic,
                'text-underline': child.underline
              }"
            >
              {{ child.text }}
            </span>
          </p>
          
          <ul v-else-if="block.type === 'list' && block.format === 'unordered'">
            <li v-for="(listItem, itemIndex) in block.children" :key="itemIndex">
              <span 
                v-for="(child, childIndex) in listItem.children" 
                :key="childIndex"
                :class="{ 'text-bold': child.bold, 'text-italic': child.italic }"
              >
                {{ child.text }}
              </span>
            </li>
          </ul>
          <ol v-else-if="block.type === 'list' && block.format === 'ordered'">
            <li v-for="(listItem, itemIndex) in block.children" :key="itemIndex">
              <span 
                v-for="(child, childIndex) in listItem.children" 
                :key="childIndex"
                :class="{ 'text-bold': child.bold, 'text-italic': child.italic }"
              >
                {{ child.text }}
              </span>
            </li>
          </ol>

        </template>
      </div>

            <div class="article-footer">
                <NuxtLink to="/" class="back-btn">← Back to Articles</NuxtLink>
            </div>

        </div>
    </div>
</template>

<style scoped>
.article-container {
    max-width: 800px;
    margin: 40px auto;
    padding: 0 20px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #333;
}

.article-card {
    background: #ffffff;
    border: 1px solid #eaeaea;
    border-radius: 10px;
    padding: 40px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.article-header {
    margin-bottom: 30px;
}

.article-title {
    font-size: 2.5rem;
    color: #2c3e50;
    margin-top: 0;
    margin-bottom: 15px;
    line-height: 1.2;
}

.article-meta {
    display: flex;
    align-items: center;
    gap: 15px;
}

.badge {
    background-color: #3498db;
    color: white;
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 0.85rem;
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    text-decoration: none;
    transition: background-color 0.2s;
}

a.badge:hover {
    background-color: #2980b9;
}

.author-text {
    color: #7f8c8d;
    font-size: 1rem;
    font-weight: 500;
}

.article-cover {
    width: 100%;
    height: 400px;
    object-fit: cover;
    border-radius: 8px;
    margin-bottom: 30px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.article-content {
    line-height: 1.8;
    font-size: 1.1rem;
    color: #444;
}

.article-content p {
    margin-bottom: 20px;
}

.article-footer {
    margin-top: 40px;
    padding-top: 20px;
    border-top: 1px solid #eaeaea;
}

.back-btn {
    display: inline-block;
    text-decoration: none;
    color: #3498db;
    font-weight: 600;
    padding: 10px 20px;
    border-radius: 6px;
    background-color: #f0f8ff;
    transition: background-color 0.2s, color 0.2s;
}

.back-btn:hover {
    background-color: #3498db;
    color: white;
}

.article-heading {
  color: #2c3e50;
  margin-top: 35px;
  margin-bottom: 15px;
  line-height: 1.3;
}

.article-content ul, 
.article-content ol {
  margin-bottom: 20px;
  padding-left: 25px;
}

.article-content li {
  margin-bottom: 8px;
}

.text-bold {
  font-weight: bold;
  color: #2c3e50;
}

.text-italic {
  font-style: italic;
}

.text-underline {
  text-decoration: underline;
}
</style>