<script setup>
import MovieCard from './components/MovieCard.vue';
import movies from './assets/movies'
import { ref, computed } from 'vue';

const query = ref('')
const searchQuery = ref('')

movies.forEach(movie => {
  delete movie.cast
  delete movie.extract
  delete movie.href
})

const filteredMovies = computed(() => {
  const filteredMovies = movies.filter(movie => JSON.stringify(movie).includes(query.value))
  if (searchQuery.value) {
    return filteredMovies.filter(movie => JSON.stringify(movie).toLowerCase().includes(searchQuery.value.toLowerCase()))
  }
  return query.value ? filteredMovies : movies
})
</script>

<template>
  <main>
    <header>
      <h1>Movie Grid</h1>
      <input type="search" v-model="searchQuery" placeholder="Search Movies">
      <div class="button-group">
        <button @click="query = ''" :class="{ active: query === '' }">Clear Filters</button>
        <button @click="query = '2021'" :class="{ active: query === '2021' }">2021</button>
        <button @click="query = '2022'" :class="{ active: query === '2022' }">2022</button>
        <button @click="query = 'Action'" :class="{ active: query === 'Action' }">Action</button>
        <button @click="query = 'Comedy'" :class="{ active: query === 'Comedy' }">Comedy</button>
        <button @click="query = 'Horror'" :class="{ active: query === 'Horror' }">Horror</button>
        <button @click="query = 'Romance'" :class="{ active: query === 'Romance' }">Romance</button>
        <button @click="query = 'Adventure'" :class="{ active: query === 'Adventure' }">Adventure</button>
        <button @click="query = 'Documentary'" :class="{ active: query === 'Documentary' }">Documentary</button>
      </div>
    </header>
    <div class="movie-grid">
      <MovieCard v-for="movie in filteredMovies" :key="movie.href" :movie="movie" />
    </div>
  </main>
</template>

<style scoped>
main {
  text-align: center;
  max-width: 800px;
  align-items: center;
  margin: 0 auto;
}

header {
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

input {
  height: 50px;
  border-radius: 6px;
  border: 1px solid rgba(2, 28, 62, .1);
  font-size: 1rem;
  padding: 0 1rem;
}

.button-group {
  display: -webkit-box;
  max-width: 100%;
  overflow: auto;
}

.button-group button {
  font-size: 1rem;
  height: 40px;
  display: grid;
  place-items: center;
  margin-right: 0.4rem;
  padding: 0 1rem;
  border: 1px solid #ffab2d;
  border-radius: 6px;
  color: #ffab2d;
  background: #fffbf4;
  font-weight: 600;
  cursor: pointer;
  transition: 0.3s;
}

.button-group button.active {
  background: #ffab2d;
  color: #fffbf4;
}

h1 {
  font-weight: 700;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem 1rem;
}

@media screen and (max-width: 1024px) {
  .movie-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* @media screen and (max-width: 600px) {
  .movie-grid {
    grid-template-columns: repeat(1, 1fr);
  }
} */
</style>
