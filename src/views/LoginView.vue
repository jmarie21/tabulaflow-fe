<script setup lang="ts">
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { reactive } from 'vue'

const errors = reactive({
  email: '',
  password: '',
})

const formData = reactive({
  email: '',
  password: '',
})

const handleLogin = async () => {
  try {
    const response = await fetch('http://localhost:5228/api/auth/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData),
    })

    const result = await response.json()

    if (!response.ok || result.errors) {
      errors.email = result.errors.email?.join('. ') || '. '
      errors.password = result.errors.password?.join(' ') || ' '
    } else {
      localStorage.setItem('auth-token', result.token)
    }
  } catch (error) {
    console.error(error)
  }
}
</script>

<template>
  <div class="min-h-screen flex justify-center items-center">
    <div class="flex flex-col border border-gray-200 shadow-lg rounded-md p-8 w-1/4">
      <div class="mb-8">
        <h1 class="text-2xl font-bold">Login</h1>
      </div>

      <form class="space-y-6" @submit.prevent="handleLogin">
        <div class="space-y-2">
          <Label class="text-sm">Email</Label>
          <Input v-model="formData.email" type="text" placeholder="Enter your email" />
          <span class="text-red-500 text-sm">{{ errors.email }}</span>
        </div>

        <div class="space-y-2">
          <Label class="text-sm">Password</Label>
          <Input v-model="formData.password" type="password" placeholder="Enter your password" />
          <span class="text-red-500 text-sm">{{ errors.password }}</span>
        </div>

        <div class="space-y-8">
          <Button class="w-full" type="submit">Login</Button>
          <p class="text-center">
            Don't have an account?
            <RouterLink
              :to="{ name: 'signup' }"
              class="hover:text-green-500 underline underline-offset-2"
              >Signup here</RouterLink
            >
          </p>
        </div>
      </form>
    </div>
  </div>
</template>
