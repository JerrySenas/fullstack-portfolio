<script setup>
import { computed } from 'vue';

import ProjectCard from './ProjectCard.vue';
import projects from "../data/projects.json"

const chunkSize = 3;
const chunkedProjects = computed(() => {
  const chunks = [];
  for (let i = 0; i < projects.length; i += chunkSize) {
    chunks.push(projects.slice(i, i + chunkSize));
  }

  return chunks;
})
</script>

<template>
  <section id="projects" class="container py-3 my-5">
    <h2 class="text-center">My Projects</h2>
    <div v-for="(group, index) in chunkedProjects" :key="index" class="row my-5">
      <ProjectCard v-for="project in group" :key="project.id" :project="project" />
    </div>
  </section>
</template>