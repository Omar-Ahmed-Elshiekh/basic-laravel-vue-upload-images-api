<script setup>

import { RouterLink } from 'vue-router';
import GuestLayout from '../components/GuestLayout.vue';
import axiosClient from '../axios';
import { ref } from 'vue';
import router from '../router.js';

const data = ref({
  email: '',
  password: '',
})

const errorMsg = ref('')

function submit(){
  axiosClient.get('sanctum/csrf-cookie').then(res=>{
    axiosClient.post('/login',data.value).then(()=>{
      router.push({name: "Home"});
    }).catch(error => {
      errorMsg.value = error.response.data.message
    }
    )
  })
}

</script>

<template>
<GuestLayout>
  <h2 class="mt-10 text-center text-2xl/9 font-bold tracking-tight text-gray-900">Login to your account</h2>

  <div v-if="errorMsg" class="py-2 my-2 px-3 rounded text-red-600 bg-red-200 border-2">
    {{ errorMsg }}
  </div>

  <div class="mt-5 sm:mx-auto sm:w-full sm:max-w-sm">
      <form @submit.prevent="submit" class="space-y-4">
        <div>
          <label for="email" class="block text-sm/6 font-medium text-gray-900">Email address</label>
          <div class="mt-2">
            <input v-model="data.email" type="email" name="email" id="email" autocomplete="email" required="" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6" />
          </div>
        </div>

        <div>
          <div class="flex items-center justify-between">
            <label for="password" class="block text-sm/6 font-medium text-gray-900">Password</label>
            <div class="text-sm">
              <a href="#" class="font-semibold text-indigo-600 hover:text-indigo-500">Forgot password?</a>
            </div>
          </div>
          <div class="mt-2">
            <input v-model="data.password" type="password" name="password" id="password" autocomplete="current-password" required="" class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6" />
          </div>
        </div>

        <div>
          <button type="submit" class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm/6 font-semibold text-white shadow-xs hover:bg-indigo-500 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Login</button>
        </div>
      </form>

      <p class="mt-10 text-center text-sm/6 text-gray-500">
        Not a member?
        {{ ' ' }}
        <RouterLink :to="{name: 'Signup'}" class="font-semibold text-indigo-600 hover:text-indigo-500">Create an account</RouterLink>
      </p>
    </div>
</GuestLayout>
</template>

<style>

</style>