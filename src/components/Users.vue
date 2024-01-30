<template>
    <div>
      <!-- Intégration du contenu Bootstrap dans le template Vue.js -->
      <NavBar />
  
      <main class="container">
        <div class="p-5 rounded">
          <h1 class="text-center">Voici les utilisateurs</h1>
          <p class="lead text-center">C'est un beau site qui affiche les users</p>
  
          <!-- Affichage de la liste des utilisateurs -->
          <center>
            <div v-if="users.length > 0" class="row row-cols-1 row-cols-md-2 g-4 mt-5">
              <div v-for="(user, index) in users" :key="index" class="col">
                <div class="card shadow">
                  <div class="card-body">
                    <h5 class="card-title">username : {{ user.username }}</h5>
                    <h5 class="card-title">Date de création : {{ formatDate(user.createdAt) }}</h5>
                    <!-- Affichage du texte "Connecté" si c'est l'utilisateur actuellement connecté -->
                    <h5 v-if="user.username === currentUser.username" class="card-title">Access token : {{ currentUser.accessToken }}</h5>
                    <h5 v-if="user.username === currentUser.username" class="text-success">Connecté</h5>
                  </div>
                </div>
              </div>
            </div>
            <div v-else class="text-muted mt-5">Aucun utilisateur trouvé.</div>
          </center>
        </div>
      </main>
    </div>
  </template>
  
  <script>
// Importez le composant NavBar
import NavBar from '@/components/NavBar';
import { baseUrl } from '../config';

export default {
  name: 'ListUser',
  components: {
    NavBar 
  },
  data() {
    return {
      users: [],
      currentUser: '',
    };
  },
  created() {
    // Effectuez la requête HTTP pour obtenir la liste des utilisateurs lors de la création du composant
    this.fetchUsers();
    // Récupérer l'utilisateur actuellement connecté
    this.currentUser = this.getCurrentUser();
  },
  methods: {
    async fetchUsers() {
      try {
        // Effectuez la requête HTTP GET vers l'API pour obtenir la liste des utilisateurs
        const response = await fetch(baseUrl);

        if (!response.ok) {
          throw new Error('Failed to fetch users');
        }

        // Convertissez la réponse en JSON
        const data = await response.json();
        // Mettez à jour la liste des utilisateurs avec les données de la réponse
        this.users = data;
      } catch (error) {
        console.error('Error fetching users:', error);
      }
    },
    getCurrentUser() {
      const cookie = document.cookie
        .split('; ')
        .find(row => row.startsWith('user='));
      if (cookie) {
        const userString = cookie.split('=')[1];
        const user = JSON.parse(userString);
        return user.user;
      }
      return '';
    },
    formatDate(dateString) {
      const options = { day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit', second: '2-digit' };
      return new Date(dateString).toLocaleDateString('fr-FR', options);
    },
  },
};
</script>
