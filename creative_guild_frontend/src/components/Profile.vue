<template>
  <div class="profile-container">
    <div class="profile" v-if="user">
      <div class="profile-info">
        <div class="profile-picture">
          <img :src="user.profile_picture" alt="Profile Picture" />
        </div>
        <div class="profile-details">
          <h2>{{ user.name }}</h2>
          <p>{{ user.email }}</p>
          <p>{{ user.phone }}</p>
          <p>{{ user.bio }}</p>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Loading user profile...</p>
    </div>
    <h1>User Albums</h1>
    <div class="albums" v-if="albums.length">
      <div class="album" v-for="album in albums" :key="album.id">
        <div class="album-image">
          <img :src="album.img" alt="Album Image" />
        </div>
        <div class="album-details">
          <h2>{{ album.title }}</h2>
          <p>{{ album.description }}</p>
          <p>{{ album.date }}</p>
        </div>
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

<style scoped>
.profile-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.profile {
  background-color: #f9f9f9;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
}

.profile-info {
  display: flex;
  align-items: center;
}

.profile-picture img {
  border-radius: 50%;
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.profile-details h2 {
  font-size: 24px;
  margin-bottom: 10px;
}

.profile-details p {
  margin-bottom: 5px;
}

.albums {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.album {
  background-color: #f9f9f9;
  border-radius: 10px;
  padding: 20px;
}

.album img {
  width: 100%;
  height: auto;
  border-radius: 10px;
}

.album-details h2 {
  font-size: 20px;
  margin-bottom: 10px;
}

.album-details p {
  margin-bottom: 5px;
}
</style>