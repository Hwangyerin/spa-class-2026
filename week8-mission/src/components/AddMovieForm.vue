<script setup>
import { ref } from 'vue'

defineProps({
  genres: { type: Array, required: true }
})

const emit = defineEmits(['submit', 'cancel'])

const form = ref({ title: '', genre: '', rating: '', review: '', poster: '' })

const handleSubmit = () => {
  if (!form.value.title.trim()) {
    alert('Please enter a title.')
    return
  }
  const ratingNum = parseFloat(form.value.rating)
  if (!form.value.rating || isNaN(ratingNum) || ratingNum < 0 || ratingNum > 10) {
    alert('Please enter a valid rating between 0 and 10.')
    return
  }
  emit('submit', { ...form.value })
}
</script>

<template>
  <div class="overlay" @click.self="$emit('cancel')">
    <div class="modal">
      <div class="modal-header">
        <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <rect x="2" y="7" width="20" height="15" rx="2"/>
          <polyline points="17 2 12 7 7 2"/>
          <line x1="2" y1="12" x2="22" y2="12"/>
          <line x1="7" y1="7" x2="7" y2="22"/>
          <line x1="17" y1="7" x2="17" y2="22"/>
        </svg>
        <span>New Movie</span>
      </div>

      <div class="form-group">
        <label>title</label>
        <input v-model="form.title" type="text" class="form-input" />
      </div>

      <div class="form-group">
        <label>genre</label>
        <input v-model="form.genre" type="text" class="form-input" list="add-genre-list" />
        <datalist id="add-genre-list">
          <option v-for="g in genres" :key="g" :value="g" />
        </datalist>
      </div>

      <div class="form-group">
        <label>rate</label>
        <div class="rate-wrap">
          <input v-model="form.rating" type="number" min="0" max="10" step="0.1" class="form-input" />
          <span class="rate-suffix">/10</span>
        </div>
      </div>

      <div class="form-group">
        <label>review</label>
        <input v-model="form.review" type="text" class="form-input" />
      </div>

      <div class="form-group">
        <label>image url</label>
        <input v-model="form.poster" type="text" class="form-input" />
      </div>

      <div class="form-actions">
        <button class="btn btn-cancel" @click="$emit('cancel')">cancle</button>
        <button class="btn btn-submit" @click="handleSubmit">create</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.18);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
  backdrop-filter: blur(3px);
}

.modal {
  background: #f7f7f7;
  border-radius: 20px;
  padding: 28px 32px 24px;
  width: 420px;
  max-width: calc(100vw - 40px);
  box-shadow: 0 8px 40px rgba(0, 0, 0, 0.14);
}

.modal-header {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.9rem;
  font-weight: 500;
  color: #1a1a1a;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 14px;
}

label {
  display: block;
  font-size: 0.78rem;
  color: #555;
  margin-bottom: 5px;
}

.form-input {
  width: 100%;
  padding: 9px 12px;
  background: #e4e4e4;
  border: none;
  border-radius: 10px;
  font-size: 0.85rem;
  color: #1a1a1a;
  outline: none;
  font-family: inherit;
  transition: background 0.15s;
}

.form-input:focus {
  background: #d8d8d8;
}

.rate-wrap {
  position: relative;
  display: flex;
  align-items: center;
}

.rate-wrap .form-input {
  padding-right: 36px;
}

.rate-suffix {
  position: absolute;
  right: 12px;
  font-size: 0.78rem;
  color: #aaa;
  pointer-events: none;
}

.form-actions {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.btn {
  flex: 1;
  padding: 10px;
  border: none;
  border-radius: 12px;
  font-size: 0.83rem;
  cursor: pointer;
  font-family: inherit;
  transition: background 0.15s;
}

.btn-cancel {
  background: #e0e0e0;
  color: #555;
}

.btn-cancel:hover {
  background: #d4d4d4;
}

.btn-submit {
  background: #d4d4d4;
  color: #1a1a1a;
  font-weight: 500;
}

.btn-submit:hover {
  background: #c8c8c8;
}
</style>
