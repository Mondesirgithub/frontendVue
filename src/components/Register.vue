<template>
    <NavBar />
    <div class="container-fluid d-flex align-items-center justify-content-center vh-100">
      <div class="form-signin" style="width: 50%;">
        <form @submit.prevent="register">
          <h1 class="h3 mb-3 fw-normal text-center">Inscription</h1>
  
          <div class="form-floating mb-3">
            <input type="text" class="form-control" v-model="username" id="floatingInput" placeholder="Username" required @input="validateForm">
            <label for="floatingInput">Username</label>
          </div>
          <div class="form-floating mb-3">
            <input type="password" class="form-control" v-model="password" id="floatingPassword" placeholder="Password" required @input="validateForm">
            <label for="floatingPassword">Mot de passe</label>
          </div>
  
          <!-- Affichage du message d'erreur -->
          <p v-if="errorMessage" class="text-danger">{{ errorMessage }}</p>
  
          <button class="btn btn-primary w-100 py-2" type="submit" :disabled="isInvalidForm">S'inscrire</button>
          <p class="mt-3 text-center">Avez-vous un compte ? <router-link to="/login">Connectez-vous !</router-link></p>
          <p class="mt-5 mb-3 text-body-secondary text-center">&copy; 2024</p>
        </form>
      </div>
    </div>
</template>

<script>
import NavBar from './NavBar.vue';
import { baseUrl } from '../config';

export default {
  name: 'RegisterForm',
  components: {
    NavBar,
  },
  data() {
    return {
      username: '',
      password: '',
      errorMessage: '',
    };
  },
  computed: {
    isInvalidForm() {
      return !this.username.trim() || !this.password.trim();
    },
  },
  methods: {
    async register() {
      try {
        const response = await fetch(`${baseUrl}/register`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ username: this.username, password: this.password }),
        });

        if (!response.ok) {
          // Si la réponse n'est pas OK, récupérer le message d'erreur
          const errorData = await response.json();
          throw new Error(errorData.message);
        }
        alert('Utilisateur crée avec succès, vous pouvez vous connecter !')
        this.$router.push('/login');
      } catch (error) {
        this.errorMessage = `${error}`;
      }
    },
    validateForm() {
      this.errorMessage = '';
    },
  },
};
</script>


<style scoped>
.text-danger {
  color: red;
}
</style>
