<script setup>
defineProps({
  project: Object
});
</script>

<template>
  <div class="col-lg-4">
    <div class="card h-100 p-3">
      <img :src="project.image" class="card-img-top" alt="...">
      <div class="card-body">
        <h4 class="card-title">{{ project.title }}</h4>
        <p class="card-text">
          {{ project.description }}
        </p>
      </div>
    </div>
  </div>
</template>