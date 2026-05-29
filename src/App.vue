<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>

    <div class="wrapper">

      <nav>
        <RouterLink to="/">Libros</RouterLink>
        <RouterLink to="/nuevoLibro">Nuevo Libro</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
    background-color: #f8f9fa;
    padding: 20px 0;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.wrapper {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
    display: flex;
    justify-content: center;
}

nav {
    display: flex;
    gap: 15px;
    justify-content: center;
}

nav a {
    display: inline-block;
    padding: 12px 24px;
    background-color: #d3d3d3;
    color: #333;
    text-decoration: none;
    border-radius: 4px;
    font-weight: bold;
    transition: all 0.3s ease;
}

nav a:hover {
    background-color: #bfbfbf;
    transform: scale(1.05);
}

nav a.router-link-active {
    background-color: #999;
    color: white;
}
</style>
