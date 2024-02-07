<template>
  <div>
    <h1>User Profile</h1>
    <button>logout</button>
    <div v-if="user">
      <h2>{{ user.name }}</h2>
      <p>Email: {{ user.email }}</p>
      <p>Phone: {{ user.phone }}</p>
      <p>Bio: {{ user.bio }}</p>
      <img :src="user.profile_picture" alt="Profile Picture" />
    </div>
    <div v-else>
      <p>Loading user profile...</p>
    </div>

    <h1>User Albums</h1>
    <div v-if="albums.length">
      <div v-for="album in albums" :key="album.id">
        <h2>{{ album.title }}</h2>
        <p>Description: {{ album.description }}</p>
        <img :src="album.img" alt="Album Image" />
        <p>Date: {{ album.date }}</p>
      </div>
    </div>
    <div v-else>
      <p>No albums found.</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

interface Album {
  id: number
  title: string
  description: string
  img: string
  date: string
}

interface User {
  id: number
  name: string
  email: string
  phone: string
  bio: string
  profile_picture: string
  albums: Album[]
}

const user = ref<User | null>(null)
const albums = ref<Album[]>([])
const route = useRoute()

const userId = Number(route.params.id)

onMounted(() => {
  fetch(`http://localhost:8888/api/user/${userId}`)
    .then((response) => response.json())
    .then((data) => {
      user.value = data.user
      albums.value = data.user.albums
    })
    .catch((error) => {
      console.error('Error fetching user profile:', error)
    })
})
</script>
