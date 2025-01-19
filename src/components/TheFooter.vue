<script setup lang="ts">
import { RouterLink } from 'vue-router'
</script>

<template>
  <nav mt-6 inline-flex gap-2 text-xl>
    <button icon-btn @click="toggleDark()">
      <div i-carbon-sun dark:i-carbon-moon />
    </button>
    <RouterLink to="/test">
      <div i-carbon-ibm-vsi-on-vpc-for-regulated-industries />
    </RouterLink>
    <a
      i-carbon-logo-github icon-btn
      rel="noreferrer"
      href="https://github.com/Rust-Magic-Master/cv_frontend"
      target="_blank"
      title="GitHub"
    />
  </nav>
</template>
