<script setup>
import { ref } from 'vue'

const props = defineProps({
  activeFilter: { type: String, required: true },
  selectedGenre: { type: String, default: null },
  genres: { type: Array, required: true }
})

const emit = defineEmits(['set-filter', 'set-genre', 'open-add'])

const showGenreDropdown = ref(false)

const toggleGenreDropdown = () => {
  showGenreDropdown.value = !showGenreDropdown.value
}

const selectGenre = (genre) => {
  emit('set-genre', genre)
  showGenreDropdown.value = false
}
</script>

<template>
  <div class="filter-bar-wrap">
    <Transition name="fade-up">
      <div v-if="showGenreDropdown" class="genre-dropdown">
        <button
          v-for="g in genres"
          :key="g"
          class="genre-option"
          :class="{ active: (selectedGenre === null && g === 'All') || selectedGenre === g }"
          @click="selectGenre(g)"
        >
          {{ g }}
        </button>
      </div>
    </Transition>

    <div class="filter-bar">
      <button
        class="filter-btn"
        :class="{ active: selectedGenre !== null }"
        @click="toggleGenreDropdown"
      >
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
          <line x1="8" y1="6" x2="21" y2="6"/>
          <line x1="8" y1="12" x2="21" y2="12"/>
          <line x1="8" y1="18" x2="21" y2="18"/>
          <circle cx="3" cy="6" r="1" fill="currentColor" stroke="none"/>
          <circle cx="3" cy="12" r="1" fill="currentColor" stroke="none"/>
          <circle cx="3" cy="18" r="1" fill="currentColor" stroke="none"/>
        </svg>
        {{ selectedGenre ? selectedGenre : 'genre' }}
      </button>

      <div class="divider"></div>

      <button
        class="filter-btn"
        :class="{ active: activeFilter === 'recent' }"
        @click="$emit('set-filter', 'recent')"
      >
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
          <circle cx="12" cy="12" r="10"/>
          <polyline points="12 6 12 12 16 14"/>
        </svg>
        recent
      </button>

      <button
        class="filter-btn"
        :class="{ active: activeFilter === 'like' }"
        @click="$emit('set-filter', 'like')"
      >
        <svg width="14" height="14" viewBox="0 0 24 24">
          <path
            d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
            :fill="activeFilter === 'like' ? 'currentColor' : 'none'"
            :stroke="activeFilter === 'like' ? 'none' : 'currentColor'"
            stroke-width="1.5"
          />
        </svg>
        like
      </button>

      <button
        class="filter-btn"
        :class="{ active: activeFilter === 'rate' }"
        @click="$emit('set-filter', 'rate')"
      >
        <svg width="14" height="14" viewBox="0 0 24 24">
          <polygon
            points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"
            :fill="activeFilter === 'rate' ? 'currentColor' : 'none'"
            :stroke="activeFilter === 'rate' ? 'none' : 'currentColor'"
            stroke-width="1.5"
          />
        </svg>
        rate
      </button>

      <button class="add-btn" @click="$emit('open-add')">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round">
          <line x1="12" y1="5" x2="12" y2="19"/>
          <line x1="5" y1="12" x2="19" y2="12"/>
        </svg>
      </button>
    </div>
  </div>
</template>

<style scoped>
.filter-bar-wrap {
  position: fixed;
  bottom: 32px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  z-index: 100;
}

.filter-bar {
  display: flex;
  align-items: center;
  gap: 2px;
  background: #e4e4e4;
  border-radius: 50px;
  padding: 5px 5px 5px 14px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.12);
}

.filter-btn {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 18px;
  border: none;
  background: transparent;
  border-radius: 50px;
  cursor: pointer;
  font-size: 0.8rem;
  color: #666;
  transition: background 0.2s, color 0.2s;
  white-space: nowrap;
  font-family: inherit;
}

.filter-btn:hover {
  background: rgba(255, 255, 255, 0.55);
  color: #1a1a1a;
}

.filter-btn.active {
  background: white;
  color: #1a1a1a;
  font-weight: 500;
}

.divider {
  width: 1px;
  height: 18px;
  background: #ccc;
  margin: 0 4px;
  flex-shrink: 0;
}

.add-btn {
  width: 38px;
  height: 38px;
  border-radius: 50%;
  border: none;
  background: #d0d0d0;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #444;
  margin-left: 4px;
  flex-shrink: 0;
  transition: background 0.15s;
}

.add-btn:hover {
  background: #c0c0c0;
}

.genre-dropdown {
  background: white;
  border-radius: 16px;
  padding: 10px;
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  max-width: 340px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.12);
}

.genre-option {
  padding: 5px 14px;
  border: 1px solid #e8e8e8;
  border-radius: 50px;
  background: white;
  cursor: pointer;
  font-size: 0.78rem;
  color: #555;
  font-family: inherit;
  transition: all 0.15s;
}

.genre-option:hover {
  background: #f0f0f0;
  border-color: #ccc;
}

.genre-option.active {
  background: #1a1a1a;
  color: white;
  border-color: #1a1a1a;
}

.fade-up-enter-active,
.fade-up-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.fade-up-enter-from,
.fade-up-leave-to {
  opacity: 0;
  transform: translateY(8px);
}
</style>
