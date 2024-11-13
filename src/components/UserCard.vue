<!-- UserCard.vue -->
<template>
    <div class="user-card">
      <h3>{{ nom }}</h3>
      <p>Email: {{ email }}</p>
      <p>Role: {{ role }}</p>
      <slot></slot> <!-- Slot principal -->
      <slot name="extra">
        <p>Hobbies: Lecture, Randonnée</p>
      </slot> <!-- Slot nommé pour contenu supplémentaire -->
      <button @click="welcomeUser">Bienvenue</button>
    </div>
  </template>
  
  
  <script>
  export default {
    props: {
      nom: {
        type: String,
        required: true
      },
      email: {
        type: String,
        required: true
      },
      role: {
        type: String,
        default: "Utilisateur"
      }
    },
    methods: {
    welcomeUser() {
      this.$emit("user-welcome", `Bienvenue, ${this.nom} !`);
    }
  }

  };
  </script>
  
  <style scoped>
  .user-card {
    border: 1px solid #ddd;
    padding: 10px;
    border-radius: 5px;
    margin: 5px 0;
  }
  </style>
  