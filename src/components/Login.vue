<!-- LoginForm.vue -->
<template>
  <NavBar />
  <div class="container-fluid d-flex align-items-center justify-content-center vh-100">
    <div class="form-signin" style="width: 50%;">
      <form @submit.prevent="login">
        <h1 class="h3 mb-3 fw-normal text-center">Connexion</h1>

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

        <button class="btn btn-primary w-100 py-2" type="submit" :disabled="isInvalidForm">Se connecter</button>
        <p class="mt-3 text-center">Vous n'avez pas de compte ? <router-link to="/register">Créez-en un ici</router-link></p>
        <p class="mt-5 mb-3 text-body-secondary text-center">&copy; 2024</p>
      </form>
    </div>
  </div>
</template>


<script>
import NavBar from './NavBar.vue';
import { baseUrl } from '../config';

export default {
  name: 'LoginForm',
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
    async login() {
      try {
        // Validation des champs
        if (this.isInvalidForm) {
          // Affichage du message d'erreur si les champs ne sont pas valides
          this.errorMessage = 'Veuillez remplir tous les champs correctement.';
          return;
        }

        const response = await fetch(`${baseUrl}/login`, {
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

        const data = await response.json();
        const user = data;

        // Enregistrez le token JWT dans le localStorage ou sessionStorage
        document.cookie = `user=${JSON.stringify(user)}; path=/;`;

        alert('Connexion réussie !');
        this.$router.push('/');
        // Redirigez l'utilisateur vers une autre page ou effectuez d'autres actions après la connexion
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

