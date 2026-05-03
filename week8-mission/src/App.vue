<script setup>
import { ref, computed, watch } from 'vue'
import MovieCard from './components/MovieCard.vue'
import AddMovieForm from './components/AddMovieForm.vue'
import EditMovieModal from './components/EditMovieModal.vue'
import FilterBar from './components/FilterBar.vue'

const GENRES = ['All', 'Action', 'Romance', 'Sci-Fi', 'Drama', 'Musical', 'Thriller', 'Comedy', 'Horror', 'Animation']

const viewMode = ref('slide')
const currentIndex = ref(0)
const slideDirection = ref('next')
const activeFilter = ref('recent')
const selectedGenre = ref(null)
const showAddForm = ref(false)
const editingMovie = ref(null)
let nextId = 7

const movies = ref([
  {
    id: 1,
    title: 'Inception',
    genre: 'Sci-Fi',
    rating: 8.8,
    review: 'A mind-bending thriller that explores the architecture of the subconscious. Christopher Nolan crafts an intricate world where reality and dreams are indistinguishable from one another.',
    date: '2010.07.16',
    likes: 0,
    poster: 'https://i.namu.wiki/i/ujkt6TPVoeYxq3q9h2yE_MckhgdJ6rOcdgnkCcs1ivWzB5hlnPBCEzatFDMApkNfWlxuCTQYH3d2SMj632Fzsg.webp'
  },
  {
    id: 2,
    title: 'About Time',
    genre: 'Romance',
    rating: 8.6,
    review: 'A heartwarming story about time travel and the importance of cherishing every ordinary moment with the people you love. Beautifully written and genuinely moving.',
    date: '2013.11.08',
    likes: 3,
    poster: 'https://i.namu.wiki/i/LzoxaAtXeLBOtibjJw34YNxQB8peseMhLmC9GLzAO01_1GOZMHDh9jDR_HbuWwbDS6jwdkkH3eV1mmeYnGvhbQ.webp'
  },
  {
    id: 3,
    title: 'The Dark Knight',
    genre: 'Action',
    rating: 9.0,
    review: "Heath Ledger's Joker defines cinematic villainy. A superhero film that transcends the genre entirely with its moral complexity and relentless tension.",
    date: '2008.07.18',
    likes: 0,
    poster: 'https://i.namu.wiki/i/tvjifOxt40saLEdUAOD1f150KXOOOti08v2KwiF4Z3NjPq01674QKLUeTNP2VOB-xQKu8hD7svp3vWoCAoa1Sg.jpg'
  },
  {
    id: 4,
    title: 'La La Land',
    genre: 'Musical',
    rating: 8.5,
    review: 'A melancholic ode to dreamers and the sacrifices they make. Gosling and Stone light up every frame of this bittersweet love story set against Hollywood.',
    date: '2016.12.09',
    likes: 1,
    poster: 'https://i.namu.wiki/i/78uTXq-Jd3ME_MYXtiyOo-qBPjwpiNF9qs1ko9YvE1BmaVagE9-h95a5Xuh0jVt6WX9sY8seQLZlU2GidF7Gcg.webp'
  },
  {
    id: 5,
    title: 'Parasite',
    genre: 'Thriller',
    rating: 9.2,
    review: "Bong Joon-ho's masterpiece. A sharp, darkly comic critique of class inequality that builds into a gripping and deeply unsettling thriller. Deserving of every award it received.",
    date: '2019.05.30',
    likes: 0,
    poster: 'https://i.namu.wiki/i/OSztJWgsdImEDUbQ5HE8rtJCF_bKjLbCUJhdLVWG6zHKcHa-rGeQJOV9KayYu91DliUfQhOFfP-o_ctAQQK-GA.webp'
  },
  {
    id: 6,
    title: 'Whiplash',
    genre: 'Drama',
    rating: 8.7,
    review: 'Intense and electrifying from start to finish. J.K. Simmons and Miles Teller deliver powerhouse performances exploring the brutal cost of chasing perfection.',
    date: '2014.10.15',
    likes: 0,
    poster: 'https://i.namu.wiki/i/B181jWp0wO1OJQVvT0utD7qw6BxIxpX4r80IXx25MG3m2Ev-GmqJZYx0Vq6eMURD6UGMD8WV7z0_oHlqZBzNuw.webp'
  },
  {
    id: 7,
    title: 'Interstellar',
    genre: 'Sci-Fi',
    rating: 9.1,
    review: 'A visually stunning epic that balances hard science with profound emotional resonance. A journey through space and time that ultimately celebrates the power of human connection.',
    date: '2014.11.06',
    likes: 0,
    poster: 'https://i.namu.wiki/i/yd6wnRIWEVMFY3wNjB-WKc0_nXstbPdiq87sfkbe3wJ8y2ZAhoZAfpebxEgc9bU1xIQh2OmD7Oi12CkFDhWbag.webp'
  },
  {
    id: 8,
    title: 'Begin Again',
    genre: 'Musical',
    rating: 8.4,
    review: 'A charming story about the healing power of music. The raw, soulful performances and the vibrant New York backdrop create a comforting and authentic cinematic experience.',
    date: '2014.08.13',
    likes: 2,
    poster: 'https://i.namu.wiki/i/Qcu4AfVpgp5ddeU8y7Sf5_XYjdHwSHUQuycZFROAEYTIkLeogeUFTaqb9YKkM0xXLNUz9LGbxD53F_J0N6hEfw.webp'
  },
  {
    id: 9,
    title: 'Everything Everywhere All at Once',
    genre: 'Action',
    rating: 8.9,
    review: 'An anarchic, creative explosion that uses the multiverse as a metaphor for family and existence. Wildly imaginative and deeply moving in its final moments.',
    date: '2022.10.12',
    likes: 0,
    poster: 'https://upload.wikimedia.org/wikipedia/en/1/1e/Everything_Everywhere_All_at_Once.jpg'
  },
  {
    id: 10,
    title: 'The Grand Budapest Hotel',
    genre: 'Drama',
    rating: 8.8,
    review: "Wes Anderson's visual symphony. Every frame is a meticulously crafted painting, wrapping a whimsical and nostalgic mystery in layers of vibrant color and symmetry.",
    date: '2014.03.20',
    likes: 1,
    poster: 'https://upload.wikimedia.org/wikipedia/ko/5/5a/%EA%B7%B8%EB%9E%9C%EB%93%9C%EB%B6%80%EB%8B%A4%ED%8E%98%EC%8A%A4%ED%8A%B8%ED%98%B8%ED%85%94_%ED%8F%AC%EC%8A%A4%ED%84%B0.jpg'
  },
  {
    id: 11,
    title: 'Knives Out',
    genre: 'Mystery',
    rating: 8.6,
    review: 'A brilliant modernization of the whodunit subgenre. Sharp-witted, stylish, and full of unexpected twists that keep you guessing until the very final scene.',
    date: '2019.11.27',
    likes: 0,
    poster: 'https://upload.wikimedia.org/wikipedia/ko/c/c4/%EB%82%98%EC%9D%B4%EB%B8%8C%EC%8A%A4%EC%95%84%EC%9B%83_%ED%8F%AC%EC%8A%A4%ED%84%B0.jpg'
  },
  {
    id: 12,
    title: 'Spider-Man: Into the Spider-Verse',
    genre: 'Animation',
    rating: 9.0,
    review: 'Revolutionary animation meets a deeply personal hero’s journey. A bold, vibrant, and incredibly stylish take on the Spider-Man mythos that feels entirely fresh.',
    date: '2018.12.12',
    likes: 5,
    poster: 'https://upload.wikimedia.org/wikipedia/en/thumb/f/fa/Spider-Man_Into_the_Spider-Verse_poster.png/250px-Spider-Man_Into_the_Spider-Verse_poster.png'
  }
])

const filteredMovies = computed(() => {
  let list = [...movies.value]

  if (selectedGenre.value && selectedGenre.value !== 'All') {
    list = list.filter(m => m.genre === selectedGenre.value)
  }

  if (activeFilter.value === 'like') {
    list = list.filter(m => m.likes > 0).sort((a, b) => b.likes - a.likes)
  } else if (activeFilter.value === 'rate') {
    list = list.sort((a, b) => b.rating - a.rating)
  } else {
    list = list.sort((a, b) => b.id - a.id)
  }

  return list
})

watch(filteredMovies, (list) => {
  if (currentIndex.value >= list.length) {
    currentIndex.value = Math.max(0, list.length - 1)
  }
})

const currentMovie = computed(() => filteredMovies.value[currentIndex.value] ?? null)

const ghostMovies = computed(() => {
  const list = filteredMovies.value
  const len = list.length
  if (len === 0) return []
  return [1, 2, 3].map(offset => {
    const idx = (currentIndex.value - offset + len) % len
    return list[idx]
  })
})

const toggleViewMode = () => {
  viewMode.value = viewMode.value === 'slide' ? 'grid' : 'slide'
}

const prev = () => {
  const len = filteredMovies.value.length
  slideDirection.value = 'prev'
  currentIndex.value = (currentIndex.value - 1 + len) % len
}

const next = () => {
  const len = filteredMovies.value.length
  slideDirection.value = 'next'
  currentIndex.value = (currentIndex.value + 1) % len
}

const setFilter = (filter) => {
  activeFilter.value = filter
  currentIndex.value = 0
}

const setGenre = (genre) => {
  selectedGenre.value = genre === 'All' ? null : genre
  currentIndex.value = 0
}

const toggleLike = (id) => {
  const movie = movies.value.find(m => m.id === id)
  if (movie) movie.likes++
}

const openEdit = (movie) => {
  editingMovie.value = { ...movie }
}

const saveEdit = (updated) => {
  const idx = movies.value.findIndex(m => m.id === updated.id)
  if (idx !== -1) movies.value[idx] = { ...movies.value[idx], ...updated }
  editingMovie.value = null
}

const addMovie = (form) => {
  movies.value.push({
    id: nextId++,
    title: form.title,
    genre: form.genre || 'Other',
    rating: parseFloat(form.rating),
    review: form.review || '',
    date: new Date().toISOString().slice(0, 10).replace(/-/g, '.'),
    likes: 0,
    poster: form.poster || ''
  })
  showAddForm.value = false
  activeFilter.value = 'recent'
  currentIndex.value = 0
}
</script>

<template>
  <div class="app">
    <!-- Header -->
    <header class="header">
      <button class="view-toggle" @click="toggleViewMode" :title="viewMode === 'slide' ? 'Grid view' : 'Slide view'">
        <!-- Grid icon (shown in slide mode, click to switch to grid) -->
        <svg v-if="viewMode === 'slide'" width="22" height="22" viewBox="0 0 24 24" fill="currentColor">
          <rect x="3" y="3" width="8" height="8" rx="1.5"/>
          <rect x="13" y="3" width="8" height="8" rx="1.5"/>
          <rect x="3" y="13" width="8" height="8" rx="1.5"/>
          <rect x="13" y="13" width="8" height="8" rx="1.5"/>
        </svg>
        <!-- Cards/slide icon (shown in grid mode, click to switch to slide) -->
        <svg v-else width="22" height="22" viewBox="0 0 26 26" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <rect x="8" y="4" width="14" height="18" rx="2.5"/>
          <rect x="4" y="7" width="14" height="18" rx="2.5" fill="white"/>
        </svg>
      </button>
      <h1 class="app-title">Movie Collection</h1>
      <span class="username">Yerin</span>
    </header>

    <!-- Main content -->
    <main class="main">

      <!-- ── Slide view ── -->
      <template v-if="viewMode === 'slide'">
        <div v-if="filteredMovies.length > 0" class="slide-view">
          <div class="slide-scene">
            <button class="arrow-btn" @click="prev">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="15 18 9 12 15 6"/>
              </svg>
            </button>

            <div class="card-stage">
              <div class="ghost g3">
                <img v-if="ghostMovies[2]?.poster" :src="ghostMovies[2].poster" class="ghost-img" />
              </div>
              <div class="ghost g2">
                <img v-if="ghostMovies[1]?.poster" :src="ghostMovies[1].poster" class="ghost-img" />
              </div>
              <div class="ghost g1">
                <img v-if="ghostMovies[0]?.poster" :src="ghostMovies[0].poster" class="ghost-img" />
              </div>
              <Transition :name="slideDirection === 'next' ? 'slide-next' : 'slide-prev'" mode="out-in">
                <MovieCard
                  v-if="currentMovie"
                  :key="currentMovie.id"
                  :movie="currentMovie"
                  @toggle-like="toggleLike"
                  @edit="openEdit"
                />
              </Transition>
            </div>

            <button class="arrow-btn" @click="next">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="9 18 15 12 9 6"/>
              </svg>
            </button>
          </div>
          <p class="counter">{{ currentIndex + 1 }}/{{ filteredMovies.length }}</p>
        </div>
        <div v-else class="empty-state">No movies match this filter.</div>
      </template>

      <!-- ── Grid view ── -->
      <template v-else>
        <div v-if="filteredMovies.length > 0" class="grid-view">
          <div class="movie-grid">
            <div
              v-for="movie in filteredMovies"
              :key="movie.id"
              class="grid-item"
            >
              <div class="card-flip">
                <!-- Front: poster only -->
                <div class="card-front">
                  <img v-if="movie.poster" :src="movie.poster" :alt="movie.title" class="thumb-img" />
                  <div v-else class="thumb-placeholder"></div>
                </div>

                <!-- Back: movie details -->
                <div class="card-back">
                  <div class="back-inner">
                    <button class="back-edit-btn" @click.stop="openEdit(movie)">
                      <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/>
                        <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/>
                      </svg>
                      Edit
                    </button>
                    <div class="back-top">
                      <div class="back-tag-row">
                        <span class="back-genre">{{ movie.genre }}</span>
                      </div>
                      <h3 class="back-title">{{ movie.title }}</h3>
                      <p class="back-review">{{ movie.review }}</p>
                    </div>
                    <div class="back-bottom">
                      <button class="back-heart-btn" @click.stop="toggleLike(movie.id)">
                        <svg width="15" height="15" viewBox="0 0 24 24">
                          <path
                            d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
                            :fill="movie.likes > 0 ? 'currentColor' : 'none'"
                            :stroke="movie.likes > 0 ? 'none' : 'currentColor'"
                            stroke-width="1.5"
                          />
                        </svg>
                        <span class="back-like-count">{{ movie.likes }}</span>
                      </button>
                      <div class="back-rating">
                        <span class="back-rating-value">{{ movie.rating }}</span>
                        <span class="back-rating-max">/10</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-else class="empty-state">No movies match this filter.</div>
      </template>

    </main>

    <FilterBar
      :active-filter="activeFilter"
      :selected-genre="selectedGenre"
      :genres="GENRES"
      @set-filter="setFilter"
      @set-genre="setGenre"
      @open-add="showAddForm = true"
    />

    <Teleport to="body">
      <AddMovieForm
        v-if="showAddForm"
        :genres="GENRES.filter(g => g !== 'All')"
        @submit="addMovie"
        @cancel="showAddForm = false"
      />
      <EditMovieModal
        v-if="editingMovie"
        :movie="editingMovie"
        :genres="GENRES.filter(g => g !== 'All')"
        @save="saveEdit"
        @cancel="editingMovie = null"
      />
    </Teleport>
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  background: radial-gradient(ellipse at 50% 0%, #1e1a30 0%, #0e0c15 55%);
  font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', Helvetica, Arial, sans-serif;
  display: flex;
  flex-direction: column;
}

/* ── Header ── */
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 40px;
  border-bottom: 1px solid #211e30;
  flex-shrink: 0;
}

.view-toggle {
  background: none;
  border: none;
  cursor: pointer;
  padding: 7px;
  color: #ede8da;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  transition: background 0.15s;
}

.view-toggle:hover {
  background: #211e30;
}

.app-title {
  font-size: 1.05rem;
  font-weight: 500;
  color: #ede8da;
  letter-spacing: 0.08em;
}

.username {
  font-size: 1rem;
  color: #8c8598;
  font-weight: 400;
}

/* ── Main ── */
.main {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px 40px 130px;
}

/* ── Slide view ── */
.slide-view {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 28px;
}

.slide-scene {
  display: flex;
  align-items: center;
  gap: 44px;
}

/* card-stage: ghost cards overflow to the left using negative position */
.card-stage {
  position: relative;
  width: 680px;
  height: 360px;
}

.ghost {
  position: absolute;
  width: 256px;
  height: 340px;
  top: 10px;
  border-radius: 18px;
  z-index: 0;
  overflow: hidden;
  background: #d8d8d8;
}

.ghost-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.g1 {
  left: -18px;
  transform: rotate(-2.5deg);
  opacity: 0.65;
  z-index: 1;
}

.g2 {
  left: -38px;
  transform: rotate(-4.5deg);
  opacity: 0.4;
  z-index: 0;
}

.g3 {
  left: -58px;
  transform: rotate(-6.5deg);
  opacity: 0.2;
  z-index: -1;
}

.arrow-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 12px;
  color: #aaa;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: color 0.2s, box-shadow 0.2s;
  flex-shrink: 0;
}

.arrow-btn:hover:not(:disabled) {
  color: #1a1a1a;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.12);
}

.arrow-btn:hover:not(:disabled) svg {
  stroke-width: 2.5;
}

.arrow-btn:disabled {
  opacity: 0.2;
  cursor: default;
}

.counter {
  font-size: 0.85rem;
  color: #aaa;
  letter-spacing: 0.06em;
}

/* ── Grid view ── */
.grid-view {
  width: 100%;
  max-width: 1100px;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 14px;
}

.grid-item {
  aspect-ratio: 3 / 4;
  cursor: pointer;
  perspective: 900px;
}

.card-flip {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.55s cubic-bezier(0.4, 0, 0.2, 1);
}

.grid-item:hover .card-flip {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  inset: 0;
  border-radius: 16px;
  overflow: hidden;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}

.card-front {
  background: #e4e4e4;
}

.card-back {
  background: white;
  transform: rotateY(180deg);
}

.thumb-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.thumb-placeholder {
  width: 100%;
  height: 100%;
  background: #d4d4d4;
}

.thumb-heart {
  position: absolute;
  top: 10px;
  right: 10px;
  background: rgba(255, 255, 255, 0.85);
  border: none;
  cursor: pointer;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #1a1a1a;
  transition: background 0.15s, transform 0.1s;
}

.thumb-heart:hover {
  background: white;
  transform: scale(1.1);
}

/* Card back content */
.back-inner {
  padding: 18px 16px;
  display: flex;
  flex-direction: column;
  height: 100%;
  box-sizing: border-box;
  position: relative;
}

.back-top {
  flex: 1;
  min-height: 0;
  overflow: hidden;
}

.back-tag-row {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 10px;
}

.back-genre {
  font-size: 0.7rem;
  background: #f0f0f0;
  padding: 3px 10px;
  border-radius: 50px;
  color: #666;
}

.back-title {
  font-size: 0.95rem;
  font-weight: 600;
  color: #1a1a1a;
  margin: 0 0 10px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.back-review {
  font-size: 0.72rem;
  color: #999;
  line-height: 1.65;
  display: -webkit-box;
  -webkit-line-clamp: 6;
  line-clamp: 6;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.back-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 12px;
  margin-top: auto;
  flex-shrink: 0;
}

.back-heart-btn {
  display: flex;
  align-items: center;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  color: #1a1a1a;
  transition: transform 0.1s;
}

.back-heart-btn:hover {
  transform: scale(1.15);
}

.back-like-count {
  font-size: 0.78rem;
  font-weight: 500;
  color: #1a1a1a;
  min-width: 12px;
}

.back-edit-btn {
  position: absolute;
  top: 14px;
  right: 14px;
  display: flex;
  align-items: center;
  gap: 4px;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 0.75rem;
  color: #888;
  padding: 0;
  font-family: inherit;
  transition: color 0.15s;
}

.back-edit-btn:hover {
  color: #1a1a1a;
}

.back-rating {
  display: flex;
  align-items: baseline;
}

.back-rating-value {
  font-size: 1.5rem;
  font-weight: 600;
  color: #1a1a1a;
  letter-spacing: -0.02em;
  line-height: 1;
}

.back-rating-max {
  font-size: 0.7rem;
  color: #bbb;
  margin-left: 2px;
}

/* ── Empty state ── */
.empty-state {
  color: #bbb;
  font-size: 0.95rem;
  padding: 60px 0;
}

/* ── Slide transitions ── */
.slide-next-enter-active,
.slide-next-leave-active,
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.slide-next-enter-from { opacity: 0; transform: translateX(30px); }
.slide-next-leave-to  { opacity: 0; transform: translateX(-30px); }
.slide-prev-enter-from { opacity: 0; transform: translateX(-30px); }
.slide-prev-leave-to  { opacity: 0; transform: translateX(30px); }
</style>
