<script setup>
defineProps({
  movie: { type: Object, required: true }
})

defineEmits(['toggle-like', 'edit'])
</script>

<template>
  <div class="movie-card">
    <div class="poster-side">
      <img v-if="movie.poster" :src="movie.poster" :alt="movie.title" class="poster-img" />
      <div v-else class="poster-placeholder">
        <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#bbb" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
          <rect x="2" y="7" width="20" height="15" rx="2"/>
          <polyline points="17 2 12 7 7 2"/>
          <line x1="2" y1="12" x2="22" y2="12"/>
          <line x1="7" y1="7" x2="7" y2="22"/>
          <line x1="17" y1="7" x2="17" y2="22"/>
        </svg>
      </div>
      <button
        class="heart-btn"
        :class="{ liked: movie.likes > 0 }"
        @click.stop="$emit('toggle-like', movie.id)"
        :aria-label="'Like'"
      >
        <svg width="18" height="18" viewBox="0 0 24 24">
          <path
            d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
            :fill="movie.likes > 0 ? '#1a1a1a' : 'none'"
            :stroke="movie.likes > 0 ? 'none' : '#1a1a1a'"
            stroke-width="1.5"
          />
        </svg>
        <span v-if="movie.likes > 0" class="heart-count">{{ movie.likes }}</span>
      </button>
    </div>

    <div class="info-side">
      <div class="info-top">
        <h2 class="movie-title">{{ movie.title }}</h2>
        <p class="movie-date">{{ movie.date }}</p>
        <p class="movie-review">
          <span class="review-label">review</span>
          {{ movie.review }}
        </p>
      </div>
      <div class="info-bottom">
        <button class="edit-btn" @click.stop="$emit('edit', movie)">
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/>
            <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/>
          </svg>
          Edit
        </button>
        <div class="rating">
          <span class="rating-value">{{ movie.rating }}</span>
          <span class="rating-max">/10</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.movie-card {
  display: flex;
  width: 100%;
  height: 100%;
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.08);
  position: relative;
  z-index: 3;
}

.poster-side {
  width: 260px;
  min-width: 260px;
  flex-shrink: 0;
  background: #e4e4e4;
  position: relative;
  overflow: hidden;
}

.poster-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.poster-placeholder {
  width: 100%;
  height: 100%;
  background: #d8d8d8;
  display: flex;
  align-items: center;
  justify-content: center;
}

.heart-btn {
  position: absolute;
  top: 14px;
  right: 14px;
  background: rgba(255, 255, 255, 0.88);
  border: none;
  cursor: pointer;
  padding: 7px 10px;
  border-radius: 50px;
  display: flex;
  align-items: center;
  gap: 4px;
  transition: background 0.15s, transform 0.1s;
  z-index: 1;
}

.heart-count {
  font-size: 0.75rem;
  font-weight: 500;
  color: #1a1a1a;
  line-height: 1;
}

.heart-btn:hover {
  background: white;
  transform: scale(1.1);
}

.info-side {
  flex: 1;
  min-width: 0;
  padding: 28px 28px 24px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background: white;
}

.info-top {
  flex: 1;
  min-height: 0;
}

.movie-title {
  font-size: 1.1rem;
  font-weight: 600;
  color: #1a1a1a;
  margin-bottom: 4px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.movie-date {
  font-size: 0.75rem;
  color: #bbb;
  margin-bottom: 16px;
  font-weight: 400;
}

.movie-review {
  font-size: 0.76rem;
  color: #999;
  line-height: 1.7;
  display: -webkit-box;
  -webkit-line-clamp: 6;
  line-clamp: 6;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.review-label {
  color: #1a1a1a;
  font-weight: 500;
  margin-right: 6px;
}

.info-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 16px;
  margin-top: 8px;
}

.edit-btn {
  display: flex;
  align-items: center;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 0.78rem;
  color: #888;
  padding: 0;
  transition: color 0.15s;
  font-family: inherit;
}

.edit-btn:hover {
  color: #1a1a1a;
}

.rating {
  display: flex;
  align-items: baseline;
}

.rating-value {
  font-size: 1.65rem;
  font-weight: 600;
  color: #1a1a1a;
  letter-spacing: -0.02em;
  line-height: 1;
}

.rating-max {
  font-size: 0.72rem;
  color: #bbb;
  margin-left: 2px;
}
</style>
