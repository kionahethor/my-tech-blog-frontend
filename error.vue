<script setup>

defineProps({
  error: Object
})

const handleError = () => clearError({ redirect: '/' })
</script>

<template>
  <NuxtLayout>
    <div class="error-container">
      <div class="error-card">
        <h1 class="error-code">{{ error.statusCode }}</h1>
        
        <h2 class="error-message">
          {{ error.statusCode === 404 ? 'Page Not Found' : 'Oops! Something went wrong.' }}
        </h2>
        
        <p class="error-description">
          {{ error.statusCode === 404 
            ? "We can't seem to find the page you're looking for. It might have been moved or deleted." 
            : error.message 
          }}
        </p>
        
        <button @click="handleError" class="home-btn">
          ← Take Me Home
        </button>
      </div>
    </div>
  </NuxtLayout>
</template>

<style scoped>
.error-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60vh;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.error-card {
  background: #ffffff;
  border: 1px solid #eaeaea;
  border-radius: 10px;
  padding: 50px 40px;
  text-align: center;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
}

.error-code {
  font-size: 6rem;
  font-weight: bold;
  color: #e74c3c;
  margin: 0;
  line-height: 1;
}

.error-message {
  font-size: 2rem;
  color: #2c3e50;
  margin-top: 10px;
  margin-bottom: 20px;
}

.error-description {
  color: #7f8c8d;
  font-size: 1.1rem;
  line-height: 1.6;
  margin-bottom: 30px;
}

.home-btn {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 12px 24px;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
}

.home-btn:hover {
  background-color: #2980b9;
}

.home-btn:active {
  transform: scale(0.98);
}
</style>