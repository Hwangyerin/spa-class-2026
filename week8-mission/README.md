### 🎬 SPA Movie Project: Movie Collection
본 프로젝트는 SPA(Single Page Application) 개발 실무 수업의 8주차 미션으로, Vue.js를 활용하여 동적인 사용자 경험을 제공하는 영화 관리 애플리케이션입니다.

### 🛠 핵심 개발 역량 (SPA Principles)
**Component-Based Architecture**: UI를 독립적인 단위로 분리하여 재사용성과 유지보수성을 높였습니다.

**Reactive Data Flow**: ref, watch를 활용하여 데이터 변화에 따라 UI가 즉각적으로 반응하는 반응형 웹을 구현했습니다.

**Props & Emit Communication**: 부모-자식 컴포넌트 간의 엄격한 데이터 흐름 제어를 통해 SPA의 복잡한 상태를 안정적으로 관리합니다.

### 📱 화면별 구성 및 기능
1. 영화 목록 대시보드 (Main)
- 동적 렌더링: 전체 영화 데이터를 그리드 및 슬라이드 레이아웃으로 시각화합니다.
- 상태 기반 UI 전환: 상단 토글 기능을 통해 사용자의 선택에 따라 실시간으로 레이아웃이 변경됩니다.
  <img width="1440" height="777" alt="image" src="https://github.com/user-attachments/assets/9b6552c5-c628-48d5-a3e2-827693530e88" />
  <img width="1440" height="777" alt="image" src="https://github.com/user-attachments/assets/948cf8ea-f815-489d-8a50-a573055f0463" />

<br>

2. 하단 인터랙션 바 (Navigation & Filter)
- 실시간 정렬: 장르별 필터링 및 평점/좋아요 순 정렬 기능을 제공하며, 현재 활성화된 필터에 대한 시각적 피드백(Active State)을 포함합니다.
  <img width="720" height="229" alt="image" src="https://github.com/user-attachments/assets/8ba2d631-d2f5-4e7e-9549-3488b3a3318c" />

<br>

3. 영화 정보 관리 모달 (Edit Form)
- 모달 시스템: overlay와 backdrop-filter를 활용하여 편집 환경을 구축했습니다.
- 양방향 데이터 바인딩: v-model을 활용한 실시간 입력값 관리와 유효성 검사 로직이 포함되어 있습니다.
  <img width="1439" height="776" alt="image" src="https://github.com/user-attachments/assets/2794c2f3-2d67-4d89-b883-2a04e3fe3ac7" />
