<script setup>
import { ref } from 'vue';

const userInfo = ref({
  name: '',
  gender: 'female',
  hobbies: [],
});
</script>

<template>
  <h1>3주차 실습 시작</h1>
  <div class="container">
    <h2>✏️ 실시간 회원가입 폼</h2>

    <div class="field">
      <label>이름</label>
      <input type="text" v-model="userInfo.name" placeholder="이름을 입력하세요" />
    </div>

    <div class="field">
      <label>성별</label>
      <label><input type="radio" name="gender" v-model="userInfo.gender" value="male" /> 남성</label>
      <label><input type="radio" name="gender" v-model="userInfo.gender" value="female" /> 여성</label>
    </div>

    <div class="field">
      <label>취미</label>
      <label><input type="checkbox" v-model="userInfo.hobbies" value="climbing" /> 클라이밍</label>
      <label><input type="checkbox" v-model="userInfo.hobbies" value="photography" /> 사진</label>
      <label><input type="checkbox" v-model="userInfo.hobbies" value="game" /> 게임</label>
    </div>

    <hr />
    <h3>실시간 데이터 반영 결과:</h3>
    <pre class="result-box">{{ userInfo }}</pre>
  </div>
</template>

<style scoped>
.container {
  padding: 30px;
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
  max-width: 420px;
  margin: 40px auto;
  border-radius: 16px;
  background: #ffffff;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
}

h1 {
  text-align: center;
  margin-bottom: 10px;
}

h2 {
  font-size: 18px;
  margin-bottom: 25px;
  text-align: center;
  color: #555;
}

.field {
  margin-bottom: 22px;
}

.field > label:first-child {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
}

/* 입력창 */
input[type="text"] {
  width: 100%;
  padding: 10px 12px;
  border-radius: 10px;
  box-sizing: border-box;
  border: 1px solid #ddd;
  transition: all 0.2s ease;
}

input[type="text"]:focus {
  border-color: #8fb0ee;
  outline: none;
  box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.15);
}

/* 라디오 & 체크박스 숨기고 버튼처럼 */
input[type="radio"],
input[type="checkbox"] {
  display: none;
}

/* 버튼 스타일 */
.field label:has(input) {
  display: inline-block;
  padding: 8px 14px;
  border-radius: 999px;
  border: 1px solid #ddd;
  margin-right: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 14px;
}

/* hover */
.field label:has(input):hover {
  background: hsl(240, 100%, 98%);
  border-color: #8fb0ee;
}

/* 선택됐을 때 */
.field input:checked + * {
  color: #fff;
}

/* 핵심: 선택 상태 */
.field label:has(input:checked) {
  background: #467ee5;
  color: white;
  border-color: #467ee5;
}

/* 구분선 */
hr {
  margin: 25px 0;
  border: none;
  border-top: 1px solid #eee;
}

/* 결과 박스 */
.result-box {
  background: #1e1e2f;
  color: #e4e4f0;
  padding: 15px;
  border-radius: 12px;
  font-size: 13px;
  line-height: 1.5;
  overflow-x: auto;
}
</style>