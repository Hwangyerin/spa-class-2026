<script setup>
import { ref } from 'vue';
import MovieCard from './components/MovieCard.vue';
const movies = ref([
  { id:1, title: '인셉션', rating: 8.8, likes: 0, poster: 'https://i.namu.wiki/i/ujkt6TPVoeYxq3q9h2yE_MckhgdJ6rOcdgnkCcs1ivWzB5hlnPBCEzatFDMApkNfWlxuCTQYH3d2SMj632Fzsg.webp' },
  { id:2, title: '어바웃타임', rating: 8.6, likes: 0, poster: 'https://i.namu.wiki/i/LzoxaAtXeLBOtibjJw34YNxQB8peseMhLmC9GLzAO01_1GOZMHDh9jDR_HbuWwbDS6jwdkkH3eV1mmeYnGvhbQ.webp' },
  { id:3, title: '다크 나이트', rating: 9.0, likes: 0, poster: 'https://i.namu.wiki/i/tvjifOxt40saLEdUAOD1f150KXOOOti08v2KwiF4Z3NjPq01674QKLUeTNP2VOB-xQKu8hD7svp3vWoCAoa1Sg.jpg' },
  { id:4, title: '라라랜드', rating: 8.5, likes: 0, poster: 'https://i.namu.wiki/i/78uTXq-Jd3ME_MYXtiyOo-qBPjwpiNF9qs1ko9YvE1BmaVagE9-h95a5Xuh0jVt6WX9sY8seQLZlU2GidF7Gcg.webp' }
]);

const handleLike = (targetId) => {
  const movie = movies.value.find(m => m.id === targetId);
  if (movie)  movie.likes++;
};

const handleDelete = (targetId) => {
  movies.value = movies.value.filter(m => m.id !== targetId);
};
</script>

<template>
  <div class="container">
    <h2>🍿 상호작용이 추가된 영화 리스트 (Props & Emit)</h2>
    <main class="movie-grid">
      <MovieCard
        v-for="m in movies"
        :key="m.id"
        :movie="m"
        @like-movie="handleLike"
        @delete-movie="handleDelete"
      />
    </main>
  </div>
</template>

<style scoped>
.container { 
  padding: 40px; 
  font-family: sans-serif; 
  max-width: 1000px; 
  margin: 0 auto; 
  background-color: #f8f9fa;
  min-height: 100vh;
}
h2 { 
  text-align: center; 
  margin-bottom: 40px; 
  color: #34495e;
  font-weight: 800;
}
.movie-grid { 
  display: grid; 
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); 
  gap: 25px; 
}
</style>