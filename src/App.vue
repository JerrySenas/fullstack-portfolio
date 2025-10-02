<script setup>
import Navbar from './components/Navbar.vue';
import About from './components/About.vue';
import Projects from './components/Projects.vue';
import Tools from './components/Tools.vue';
import Contacts from './components/Contacts.vue';
import Footer from './components/Footer.vue';
</script>

<template>
  <div>
    <Navbar />
    <About />
    <Projects />
    <Tools />
    <Contacts />
    <Footer />
  </div>
</template>

<style scoped>
div {
  background-color: #d2738a;
}
</style>
