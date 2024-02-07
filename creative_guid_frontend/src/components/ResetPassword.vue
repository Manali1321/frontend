<template>
  <div class="wrapper-signin">
    <img src="../../cg-logo.png" alt="Logo" class="logo" />
    <h1 class="primary-font">Password Reset Request</h1>
    <h2 class="secondary-font">
      Have an Account?
      <router-link class="cg-link" to="/">Login</router-link>
    </h2>
    <div class="form-container">
      <form @submit.prevent="handleSubmit" class="login-form">
        <div class="form-field-wrapper">
          <label for="email" class="secondary-font">Email</label>
          <input
            type="email"
            id="email"
            v-model="formData.email"
            placeholder="smith@gmail.com"
            class="cg-input"
            required
          />
        </div>
        <div class="secondary-font cg-button-wrapper">
          <button type="submit" class="cg-button">Submit Reset Request</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
import { ref } from 'vue'

const router = useRouter()

const formData = ref({
  email: '',
})

const handleSubmit = () => {
  router.push('/change_password')
}

</script>
