<template>
  <NuxtLayout>
    <Header />
    <div class="container">
      <NuxtPage />
    </div>
  </NuxtLayout>
</template>

<script setup>
import Header from '~/components/Header.vue';
import '~/assets/style.css';
</script>
