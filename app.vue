<script setup>
useHead({
  title: 'My Tech Blog',
  meta: [
    { name: 'description', content: 'A full-stack blog built with Nuxt and Strapi.' }
  ]
})
</script>

<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>