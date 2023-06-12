<template>
  <header class="shadow-md p-4 mx-auto bg-sky-500 text-white">
    <ul>
      <li>
        <NuxtLink to="/">Home</NuxtLink>
      </li>
    </ul>
  </header>

  <main class="container p-4 max-w-full">
    <slot />
  </main>

  <footer>

  </footer>
</template>

<script setup>
</script>