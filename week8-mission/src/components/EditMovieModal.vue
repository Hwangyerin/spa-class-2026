<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  movie: { type: Object, required: true },
  genres: { type: Array, required: true }
})

const emit = defineEmits(['save', 'cancel'])

const form = ref({ ...props.movie })

watch(() => props.movie, (val) => {
  form.value = { ...val }
})

const handleSave = () => {
  const ratingNum = parseFloat(form.value.rating)
  if (!form.value.title.trim()) {
    alert('Title cannot be empty.')
    return
  }
  if (isNaN(ratingNum) || ratingNum < 0 || ratingNum > 10) {
    alert('Please enter a valid rating between 0 and 10.')
    return
  }
  emit('save', { ...form.value, rating: ratingNum })
}
</script>

<template>
  <div class="overlay" @click.self="$emit('cancel')">
    <div class="modal">
      <div class="modal-header">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/>
          <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/>
        </svg>
        <span>Edit</span>
      </div>

      <div class="form-group">
        <label>title</label>
        <input v-model="form.title" type="text" class="form-input" />
      </div>

      <div class="form-group">
        <label>rate</label>
        <div class="rate-wrap">
          <input v-model="form.rating" type="number" min="0" max="10" step="0.1" class="form-input" />
          <span class="rate-suffix">/10</span>
        </div>
      </div>

      <div class="form-group">
        <label>genre</label>
        <select v-model="form.genre" class="form-select">
          <option v-for="g in genres" :key="g" :value="g">{{ g }}</option>
        </select>
      </div>

      <div class="form-group">
        <label>review</label>
        <textarea v-model="form.review" class="form-textarea" rows="4"></textarea>
      </div>

      <div class="form-group">
        <label>image url</label>
        <input v-model="form.poster" type="text" class="form-input" />
      </div>

      <div class="form-actions">
        <button class="btn btn-cancel" @click="$emit('cancel')">cancle</button>
        <button class="btn btn-save" @click="handleSave">save</button>
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

.form-select {
  width: 100%;
  padding: 9px 12px;
  background: #e4e4e4;
  border: none;
  border-radius: 10px;
  font-size: 0.85rem;
  color: #1a1a1a;
  outline: none;
  font-family: inherit;
  cursor: pointer;
  appearance: auto;
}

.form-select:focus {
  background: #d8d8d8;
}

.form-textarea {
  width: 100%;
  padding: 9px 12px;
  background: #e4e4e4;
  border: none;
  border-radius: 10px;
  font-size: 0.85rem;
  color: #1a1a1a;
  outline: none;
  font-family: inherit;
  resize: vertical;
  line-height: 1.6;
  transition: background 0.15s;
}

.form-textarea:focus {
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

.btn-save {
  background: #d4d4d4;
  color: #1a1a1a;
  font-weight: 500;
}

.btn-save:hover {
  background: #c8c8c8;
}
</style>
