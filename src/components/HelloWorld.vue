<template>
  <div>
    <!-- Intégration du contenu Bootstrap dans le template Vue.js -->
    <NavBar />

    <main class="container">
      <div class="bg-body-tertiary p-5 rounded">
        <h1>Application de gestion utilisateurs</h1>
        <p class="lead">C'est une app qui permet d'afficher les users, créer des users et connecter les users</p>
        <router-link to="/users" class="btn btn-lg btn-primary text-white">Liste des users &raquo;</router-link>
      </div>
    </main>
  </div>
</template>


<script>
// Importez le composant NavBar
import NavBar from '@/components/NavBar';

export default {
  components: {
    NavBar // Enregistrez le composant NavBar comme un composant enfant
  }
};
</script>