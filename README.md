## Image virtualization(windowing)

### 기술스택

- **react-query**: 무한 스크롤 데이터 관리 (API 호출 및 캐싱)
- **react-virtual**: 이미지 리스트 가상화(virtualization) 처리

---

### 동작 원리 및 구조

1. **데이터 패칭 및 무한 스크롤**

   - `react-query`의 `useInfiniteQuery`를 사용해 이미지 리스트를 페이지 단위로 비동기 요청합니다.
   - 스크롤이 리스트 하단에 도달하면 자동으로 다음 페이지를 요청하여 데이터를 추가로 불러옵니다.

2. **가상 스크롤(윈도잉) 처리**

   - `react-virtual`의 `useVirtualizer`를 활용해 실제로 화면에 보이는 이미지 행(row)만 렌더링합니다.
   - 스크롤 위치에 따라 필요한 행만 동적으로 생성/제거하여, 수천 개의 이미지도 빠르게 렌더링할 수 있습니다.
   - 각 행에는 4개의 이미지를 그리드로 배치합니다.

3. **렌더링 구조**

   - 전체 이미지는 1차원 배열로 관리하며, 4개씩 묶어 행 단위로 가상화합니다.
   - 각 행은 절대 위치로 배치되어, 스크롤 시 자연스럽게 이어집니다.
   - 이미지가 아직 로딩되지 않은 행에는 'Loading...' 메시지를 표시합니다.

4. **overscan(오버스캔) 옵션**
   - `react-virtual`의 `overscan`은 실제로 화면에 보이는 영역 외에 추가로 미리 렌더링할 행(row) 개수를 의미합니다.
   - 스크롤 시 바로 다음/이전 행이 미리 렌더링되어 있어, 빠르고 부드러운 스크롤 경험을 제공합니다.
   - 값이 너무 작으면 스크롤 시 빈 공간이 보일 수 있고, 너무 크면 렌더링 성능이 저하될 수 있습니다.

---

### 주요 장점

- **성능 최적화**: 가상화로 인해 렌더링 DOM 수가 최소화되어, 대용량 이미지 리스트도 부드럽게 동작합니다.
- **확장성**: API 구조와 UI가 분리되어 있어, 다양한 이미지 소스 및 레이아웃에 쉽게 적용할 수 있습니다.
- **사용성**: 무한 스크롤과 빠른 반응성으로 사용자 경험이 우수합니다.

---

### 실행 방법

1. 의존성 설치
   ```bash
   npm install
   ```
2. 개발 서버 실행
   ```bash
   npm run dev
   ```

---

### 참고

- API 서버 주소 및 엔드포인트는 코드 내 상수(`BASE_URL`, `API_URL`)로 관리됩니다.
