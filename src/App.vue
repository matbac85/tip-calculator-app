<template>
  <main>
    <Logo />
    <Form />
  </main>
</template>

<script setup>
import Form from './components/Form.vue'
import Logo from './components/Logo.vue'
</script>

<style scoped>
main {
  background-color: var(--neutral-300);
  min-height: 100dvh;
  padding-top: 3.125rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2.5rem;
  box-sizing: border-box;
}

@media (min-width: 37.5rem) {
  main {
    padding-inline: 5rem;
  }
}

@media (min-width: 70rem) {
  main {
    gap: 5.5rem;
    padding: 0;
    justify-content: center;
  }
}
</style>
