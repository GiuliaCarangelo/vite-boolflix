<template>
  <div id="app">
    <input type="text" id="searchBar" placeholder="Search..." @keyup="handleKeyUp">
    <div v-if="!isLoading">
      <!-- Mostra i dati qui -->
      <ul>
        <li v-for="item in data" :key="item.id">{{ item.title }}</li>
      </ul>
    </div>
    <div v-else>
      Caricamento dati...
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      data: null,
      isLoading: false,
      searchQuery: "",
    };
  },
  // mounted() {
  //   this.loadData();
  // },
  methods: {
    handleKeyUp(event) {
      console.warn(event.target.value)
      this.searchQuery = event.target.value;
      console.log(this.searchQuery)
      this.searchQuery && this.loadData();
    },
    loadData() {
      const apiUrl = "https://api.themoviedb.org/3/search/movie";
      const apiKey = "a39179d3d58203d452e8dea06f2f6bbf";
      const headers = {
        'Authorization': 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJhMzkxNzlkM2Q1ODIwM2Q0NTJlOGRlYTA2ZjJmNmJiZiIsInN1YiI6IjY1ODJjN2E1ZjE3NTljM2Y1MTEwY2E3OCIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.bi8-eVvbYD3iCXgQWGcRb5k06u6Fl_CDpup0ALuZA7w', // Sostituisci con il tuo token di accesso
        'Content-Type': 'application/json',
      };
      this.isLoading = true;
      axios.get(apiUrl, { headers, params: { api_key: apiKey, query: this.searchQuery } })
        .then(response => {
          this.data = response.data.results;
          this.isLoading = false;
            if (this.searchQuery === ""){
          this.searchQuery = "";
      }
        })
        .catch(error => {
          console.error('Errore durante il recupero dei dati:', error);
        });
    },
    emptySearchContent(){
    }
  },
};
</script>

<style>
/* Stili per App.vue */
</style>