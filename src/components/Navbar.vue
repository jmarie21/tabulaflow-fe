<script setup lang="ts">
import { Button } from './ui/button'
</script>

<template>
  <header>
    <nav class="w-full fixed top-0 flex items-center justify-between py-4 px-64">
      <div>
        <h1 class="text-3xl font-bold">TabulaFlow</h1>
      </div>

      <div class="space-x-4">
        <RouterLink :to="{ name: 'login' }">
          <Button> Login </Button>
        </RouterLink>
        <RouterLink :to="{ name: 'signup' }">
          <Button variant="outline"> Signup </Button>
        </RouterLink>
      </div>
    </nav>
  </header>
</template>
