<script setup>
import { ref } from 'vue';

// 검색 관련 상태
const searchInput = ref('');
const searchKeyword = ref('');
const currentTab = ref('전체');

// 모달 관련 상태
const isModalOpen = ref(false);
const selectedMovie = ref(null);

// 영화 데이터
const movies = ref([
  {
    id: 1,
    title: '다크 나이트',
    genre: '액션',
    rating: 9.5,
    poster: 'https://i.namu.wiki/i/tvjifOxt40saLEdUAOD1f150KXOOOti08v2KwiF4Z3NjPq01674QKLUeTNP2VOB-xQKu8hD7svp3vWoCAoa1Sg.jpg'
  },
  {
    id: 2,
    title: '인터스텔라',
    genre: 'SF',
    rating: 10.0,
    poster: 'https://i.namu.wiki/i/z6D_GF913xg5EKvc64JYvBqVoVgYzDwIz3Zs91LgKb_tOE7gxP9_8Ytjv1i9lxNgpFmtxaJQ7yIvsT8SyUbv3Q.webp'
  },
  {
    id: 3,
    title: '어바웃 타임',
    genre: '로맨스',
    rating: 7.5,
    poster: 'https://i.namu.wiki/i/LzoxaAtXeLBOtibjJw34YNxQB8peseMhLmC9GLzAO01_1GOZMHDh9jDR_HbuWwbDS6jwdkkH3eV1mmeYnGvhbQ.webp'
  },
  {
    id: 4,
    title: '인셉션',
    genre: 'SF',
    rating: 9.0,
    poster: 'https://i.namu.wiki/i/O2uuv7bO0Hc33rLHS7t3OGhc5_guUiIyY6VThmyKSP8lC1kwtN6vS8KsiUIbda5n46DAVy7_Z2_c2KRR_mUYEw.webp'
  },
  {
    id: 5,
    title: '라라랜드',
    genre: '로맨스',
    rating: 8.0,
    poster: 'https://i.namu.wiki/i/78uTXq-Jd3ME_MYXtiyOo-qBPjwpiNF9qs1ko9YvE1BmaVagE9-h95a5Xuh0jVt6WX9sY8seQLZlU2GidF7Gcg.webp'
  }
]);

// 검색 실행
const handleSearch = () => {
  searchKeyword.value = searchInput.value;
};

// 모달 열기
const openModal = (movie) => {
  selectedMovie.value = movie;
  isModalOpen.value = true;
};

// 모달 닫기
const closeModal = () => {
  isModalOpen.value = false;
  selectedMovie.value = null;
};

// 영화 삭제
const deleteMovie = (targetId) => {
  movies.value = movies.value.filter((movie) => movie.id !== targetId);
  closeModal();
};
</script>

<template>
  <div class="container">
    <h1>🍿 영화 데이터베이스</h1>

    <div class="search-area">
      <input
        type="text"
        v-model="searchInput"
        @keyup.enter="handleSearch"
        placeholder="영화 제목을 입력하고 엔터를 눌러주세요"
      />
      <button @click="handleSearch">검색</button>
    </div>

    <div class="tabs">
      <button
        @click="currentTab = '전체'"
        :class="{ active: currentTab === '전체' }"
      >
        전체
      </button>
      <button
        @click="currentTab = '액션'"
        :class="{ active: currentTab === '액션' }"
      >
        액션
      </button>
      <button
        @click="currentTab = 'SF'"
        :class="{ active: currentTab === 'SF' }"
      >
        SF
      </button>
      <button
        @click="currentTab = '로맨스'"
        :class="{ active: currentTab === '로맨스' }"
      >
        로맨스
      </button>
    </div>

    <div class="movie-grid">
      <div
        v-for="movie in movies"
        :key="movie.id"
        class="card"
        v-show="
          (currentTab === '전체' || movie.genre === currentTab) &&
          movie.title.includes(searchKeyword)
        "
      >
        <img :src="movie.poster" :alt="movie.title + ' 포스터'" />
        <h2>{{ movie.title }}</h2>
        <p>⭐ {{ movie.rating }} / {{ movie.genre }}</p>

        <div class="badge-area">
          <span v-if="movie.rating >= 9">👑 명작</span>
          <span v-else-if="movie.rating >= 8">👍 추천</span>
          <span v-else>🎬 킬링타임</span>
        </div>

        <button @click="openModal(movie)">상세보기</button>
      </div>
    </div>

    <div v-if="isModalOpen" class="modal-bg" @click="closeModal">
      <div class="modal-content" @click.stop>
        <h2>{{ selectedMovie?.title }} 상세정보</h2>
        <img
          :src="selectedMovie?.poster"
          :alt="selectedMovie?.title + ' 포스터'"
        />
        <p>장르: {{ selectedMovie?.genre }}</p>
        <p>평점: ⭐ {{ selectedMovie?.rating }}</p>
        <button @click="closeModal">닫기</button>
        <button @click="deleteMovie(selectedMovie.id)">삭제</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  font-family: sans-serif;
}

.search-area {
  margin: 20px 0;
}

.search-area input {
  padding: 8px;
  width: 220px;
  margin-right: 6px;
}

.search-area button {
  padding: 8px 15px;
  cursor: pointer;
}

.tabs {
  margin: 20px 0;
}

.tabs button {
  margin: 0 5px;
  padding: 8px 14px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  background-color: #eee;
}

.tabs button.active {
  background-color: #222;
  color: white;
}

.movie-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  margin-top: 30px;
}

.card {
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 12px;
  width: 200px;
  text-align: center;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  background: white;
}

.card img {
  width: 100%;
  border-radius: 8px;
}

.badge-area {
  margin: 10px 0;
  font-weight: bold;
}

.modal-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 12px;
  width: 320px;
  text-align: center;
}

.modal-content img {
  width: 100%;
  border-radius: 8px;
  margin-bottom: 10px;
}

.modal-content button {
  margin: 5px;
  padding: 8px 14px;
  cursor: pointer;
}
</style>