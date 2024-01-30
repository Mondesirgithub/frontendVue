<template>
    <nav class="navbar navbar-expand-md navbar-dark bg-dark mb-4">
      <div class="container-fluid">
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarCollapse" aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <ul class="navbar-nav me-auto mb-2 mb-md-0">
            <li class="nav-item">
              <router-link to="/" class="nav-link" :class="{ active: $route.path === '/' }" aria-current="page" exact>Home</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/login" class="nav-link" :class="{ active: $route.path === '/login' }">Login</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/register" class="nav-link" :class="{ active: $route.path === '/register' }">Register</router-link>
            </li>
          </ul>
          <!-- Affichage du nom d'utilisateur -->
          <span class="navbar-text me-3 text-white" v-if="currentUser">
            {{ currentUser.username }}
          </span>
          <!-- Bouton de déconnexion -->
          <button class="btn btn-outline-light" v-if="currentUser" @click="logout">Déconnexion</button>
        </div>
      </div>
    </nav>
  </template>
  
  <script>
  export default {
    name: 'NavBar',
    computed: {
      // Récupérer les informations de l'utilisateur actuel à partir du cookie "user"
      currentUser() {
        const cookie = document.cookie
          .split('; ')
          .find(row => row.startsWith('user='));
        if (cookie) {
          const userString = cookie.split('=')[1];
          return JSON.parse(userString).user;
        }
        return null;
      },
    },
    methods: {
      // Méthode de déconnexion
      logout() {
        // Supprimer le cookie "user"
        document.cookie = 'user=; Max-Age=-99999999;';
        // Rediriger l'utilisateur vers la page de connexion
        this.$router.push('/login');
      },
    },
  };
  </script>
  
  <style scoped>
  /* Style pour le lien actif */
  .active {
    color: blue; /* Changer la couleur du texte */
  }
  </style>
  