<script setup>
import { ref, onMounted } from 'vue'

const user = ref(null)
const loading = ref(false)
const error = ref('')

async function fetchUser() {
  loading.value = true
  error.value = ''

  try {
    const response = await fetch('https://randomuser.me/api/')
    const data = await response.json()
    user.value = data.results[0]
  } catch (err) {
    error.value = 'Failed to load user data.'
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchUser()
})
</script>

<template>
  <div class="container">
    <h1>Random User Generator</h1>

    <button @click="fetchUser">Get New User</button>

    <p v-if="loading">Loading data...</p>

    <p v-else-if="error">{{ error }}</p>

    <div v-else-if="user" class="user-card">
      <img
        :src="user.picture.large"
        :alt="user.name.first"
      />

      <h2>
        {{ user.name.first }}
        {{ user.name.last }}
      </h2>

      <p>{{ user.email }}</p>
    </div>
  </div>
</template>

<style>
body {
  background-color: #f4f6f8;
  font-family: Arial, sans-serif;
  margin: 0;
}

.container {
  text-align: center;
  padding: 2rem;
}

h1 {
  color: #2c3e50;
}

button {
  padding: 10px 20px;
  margin: 20px 0;
  cursor: pointer;
  border: none;
  background-color: #42b883;
  color: white;
  border-radius: 5px;
}

button:hover {
  background-color: #369870;
}

.user-card {
  background: white;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}

.user-card img {
  border-radius: 50%;
}
</style>