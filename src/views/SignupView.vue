<script setup lang="ts">
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import router from '@/router'
import { reactive, ref } from 'vue'

const loading = ref(false)

const errors = reactive({
  fullName: '',
  email: '',
  password: '',
})

const formData = reactive({
  fullName: '',
  email: '',
  password: '',
})

const handleSignup = async () => {
  try {
    loading.value = true
    const response = await fetch('http://localhost:5228/api/auth/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData),
    })

    const result = await response.json()
    if (!response.ok) {
      console.error('Error signing up')
      if (result.errors) {
        errors.fullName = result.errors.fullName?.join(' ') || ''
        errors.email = result.errors.email?.join('. ') || '. '
        errors.password = result.errors.password?.join(' ') || ''
      }
    } else {
      router.push('/login')
    }
    console.log(result.errors)
  } catch (error) {
    console.error(error)
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="min-h-screen flex justify-center items-center">
    <div class="flex flex-col border border-gray-200 shadow-lg rounded-md p-8 w-1/4">
      <div class="mb-8">
        <h1 class="text-2xl font-bold">Signup</h1>
      </div>

      <form class="space-y-6" @submit.prevent="handleSignup">
        <div class="space-y-2">
          <Label class="text-sm">Full Name</Label>
          <Input v-model="formData.fullName" type="text" placeholder="Enter your full name" />
          <span class="text-red-500 text-sm">{{ errors.fullName }}</span>
        </div>

        <div class="space-y-2">
          <Label class="text-sm">Email</Label>
          <Input v-model="formData.email" type="email" placeholder="Enter your email" />
          <span class="text-red-500 text-sm">{{ errors.email }}</span>
        </div>

        <div class="space-y-2">
          <Label class="text-sm">Password</Label>
          <Input v-model="formData.password" type="password" placeholder="Enter your password" />
          <span class="text-red-500 text-sm">{{ errors.password }}</span>
        </div>

        <div class="space-y-8">
          <Button :disabled="loading" class="w-full" type="submit">Signup</Button>
          <p class="text-center">
            Already have an account?
            <RouterLink
              :to="{ name: 'login' }"
              class="hover:text-green-500 underline underline-offset-2"
              >Login here</RouterLink
            >
          </p>
        </div>
      </form>
    </div>
  </div>
</template>
