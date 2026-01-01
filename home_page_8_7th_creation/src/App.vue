<script setup lang="ts">
import { ref } from 'vue'

const isSearchActive = ref(false)
const searchQuery = ref('')

const handleSearchClick = () => {
  if (!isSearchActive.value) {
    isSearchActive.value = true
  }
}

const handleSearchBlur = () => {
  // 입력이 비어 있으면 다시 placeholder 상태로 복귀
  if (searchQuery.value === '') {
    isSearchActive.value = false
  }
}
</script>

<template>
  <div class="page">
    <!-- Top-left text block from Figma
         position: (38, 29), size: (131 x 69)
         font: Space Mono, regular, 35px, #000000
         stroke: 1px outside (#000000) -->
    <div class="top-text">
      <span class="top-text__inner">
        LATENT
      </span>
    </div>

    <!-- 상단 '모든 제품' 배경 바 (X:1047, Y:29, W:542, H:69, fill:#D9D9D9) -->
    <div class="all-products-bar"></div>

    <!-- 상단 '모든 제품' 강조 직사각형 (X:1047, Y:29, W:128, H:69, fill:#EDE9E9) -->
    <div class="all-products-pill">
      <svg
        class="all-products-pill__icon"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="m19.5 8.25-7.5 7.5-7.5-7.5"
        />
      </svg>
      <span class="all-products-pill__text">모든 제품</span>
    </div>

    <!-- 상단 검색어 입력 placeholder / 검색바 (클릭 시 전환) -->
    <div class="search-placeholder" @click="handleSearchClick">
      <template v-if="!isSearchActive">
        <svg
          class="search-placeholder__icon"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="2"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="m21 21-5.197-5.197m0 0A7.5 7.5 0 1 0 5.196 5.196a7.5 7.5 0 0 0 10.607 10.607Z"
          />
        </svg>
        <span class="search-placeholder__text">검색어 입력</span>
      </template>
      <input
        v-else
        class="search-input"
        type="text"
        v-model="searchQuery"
        @blur="handleSearchBlur"
      />
    </div>

    <!-- Figma 직사각형 + '회원가입' 텍스트 (X:1622, Y:41, W:118, H:45) -->
    <div class="top-rect">
      <span class="top-rect__text">회원가입</span>
    </div>

    <!-- Figma 검은 직사각형 + '로그인' 텍스트 (X:1774, Y:41, W:118, H:45) -->
    <div class="top-rect--black">
      <span class="top-rect--black__text">로그인</span>
    </div>


    <!-- Single category icon from Figma (X:40, Y:189, 69x69, #D9D9D9) -->
    <div class="single-category-icon"></div>

    <!-- Second category icon from Figma (X:177, Y:189, 69x69, #D9D9D9) -->
    <div class="second-category-icon"></div>

    <!-- Separator line next to the first icon (X:137, Y:189, H:107, #EDE9E9) -->
    <div class="single-category-line"></div>

    <!-- Separator line between first and second icons (X:292, Y:189, H:107, #EDE9E9) -->
    <div class="second-category-line"></div>

    <!-- Third category icon from Figma (X:331, Y:189, 69x69, #D9D9D9) -->
    <div class="third-category-icon"></div>

    <!-- Separator line to the right of third icon (X:441, Y:189, H:107, #EDE9E9) -->
    <div class="third-category-line"></div>

    <!-- Fourth category icon from Figma (X:482, Y:189, 69x69, #D9D9D9) -->
    <div class="fourth-category-icon"></div>

    <!-- Separator line to the right of fourth icon (X:594, Y:189, H:107, #EDE9E9) -->
    <div class="fourth-category-line"></div>

    <!-- Fifth category icon from Figma (X:656, Y:189, 69x69, #D9D9D9) -->
    <div class="fifth-category-icon"></div>

    <!-- Separator line to the right of fifth icon (X:789, Y:189, H:107, #EDE9E9) -->
    <div class="fifth-category-line"></div>

    <!-- Sixth category icon from Figma (X:853, Y:189, 69x69, #D9D9D9) -->
    <div class="sixth-category-icon"></div>

    <!-- Separator line to the right of sixth icon (X:988, Y:189, H:107, #EDE9E9) -->
    <div class="sixth-category-line"></div>

    <!-- Seventh category icon from Figma (X:1061, Y:189, 69x69, #D9D9D9) -->
    <div class="seventh-category-icon"></div>

    <!-- Separator line to the right of seventh icon (X:1206, Y:189, H:107, #EDE9E9) -->
    <div class="seventh-category-line"></div>

    <!-- Eighth category icon from Figma (X:1270, Y:189, 69x69, #D9D9D9) -->
    <div class="eighth-category-icon"></div>

    <!-- Separator line to the right of eighth icon (X:1408, Y:189, H:107, #EDE9E9) -->
    <div class="eighth-category-line"></div>

    <!-- Ninth category icon from Figma (X:1454, Y:189, 69x69, #D9D9D9) -->
    <div class="ninth-category-icon"></div>

    <!-- Separator line to the right of ninth icon (X:1573, Y:189, H:107, #EDE9E9) -->
    <div class="ninth-category-line"></div>

    <!-- Tenth category icon from Figma (X:1612, Y:189, 69x69, #EDE9E9) -->
    <div class="tenth-category-icon"></div>

    <!-- Separator line to the right of tenth icon (X:1719, Y:189, H:107, #EDE9E9) -->
    <div class="tenth-category-line"></div>

    <!-- '신제품' label under the first icon (X:40, Y:266, W:69, H:30) -->
    <div class="single-category-label">
      <span class="single-category-label__text">신제품</span>
    </div>

    <!-- '겨울 이벤트' label under the second icon (X:153, Y:266, W:123, H:30) -->
    <div class="second-category-label">
      <span class="second-category-label__text">겨울 이벤트</span>
    </div>

    <!-- '수납 가구' label under the third icon (X:316, Y:266, W:100, H:30) -->
    <div class="third-category-label">
      <span class="third-category-label__text">수납 가구</span>
    </div>

    <!-- '수납 용품' label under the fourth icon (X:468, Y:266, W:100, H:30) -->
    <div class="fourth-category-label">
      <span class="fourth-category-label__text">수납 용품</span>
    </div>

    <!-- '침대 / 매트리스' label under the fifth icon (X:610, Y:266, W:161, H:30) -->
    <div class="fifth-category-label">
      <span class="fifth-category-label__text">침대 / 매트리스</span>
    </div>

    <!-- '소파 / 암체어' label under the sixth icon (X:819, Y:266, W:138, H:30) -->
    <div class="sixth-category-label">
      <span class="sixth-category-label__text">소파 / 암체어</span>
    </div>

    <!-- '식탁 / 테이블 / 의자' label under the seventh icon (X:1007, Y:266, W:178, H:30) -->
    <div class="seventh-category-label">
      <span class="seventh-category-label__text">식탁 / 테이블 / 의자</span>
    </div>

    <!-- '책상/사무용의자' label under the eighth icon (X:1220, Y:266, W:170, H:30) -->
    <div class="eighth-category-label">
      <span class="eighth-category-label__text">책상/사무용의자</span>
    </div>

    <!-- '주방가구' label under the ninth icon (X:1443, Y:266, W:92, H:30) -->
    <div class="ninth-category-label">
      <span class="ninth-category-label__text">주방가구</span>
    </div>

    <!-- '주방 용품' label under the tenth icon (X:1601, Y:266, W:92, H:30) -->
    <div class="tenth-category-label">
      <span class="tenth-category-label__text">주방 용품</span>
    </div>

    <!-- Top navigation + divider grouped in same grid container -->
    <div class="nav-wrapper">
      <!-- Top navigation menu: horizontal flex row -->
      <nav class="nav-menu">
        <span class="nav-item nav-item--label">카테고리</span>
        <span class="nav-item">공간별 추천</span>
        <span class="nav-item">가성비 상품</span>
        <span class="nav-item">멤버십</span>
        <span class="nav-item">플래너</span>
        <span class="nav-item">크리스마스</span>
        <span class="nav-item nav-item--more">더보기</span>
      </nav>

      <!-- Divider line under navigation (Figma: vector path)
           uses same width & horizontal padding as nav items -->
      <div class="nav-divider">
        <svg
          class="nav-divider__stroke"
          viewBox="0 0 902 1"
          xmlns="http://www.w3.org/2000/svg"
        >
          <!-- line start/end align with '더보기' text container edges -->
          <line x1="0" y1="0.5" x2="902" y2="0.5" />
        </svg>
      </div>
    </div>

    <!-- Category section: horizontal scroll area -->
    <section class="category-section">
      <div class="category-scroll">
        <!-- Rectangle block
             position: (40, 189), size: (69 x 69), radius: 50, fill: #D9D9D9 -->
        <div class="rect-block"></div>

        <!-- Second vector block
             position: (225, 189), size: (69 x 69), radius: 50, fill: #D9D9D9 -->
        <div class="rect-block--vector"></div>

        <!-- Third block
             position: (410, 189), size: (69 x 69), radius: 59, fill: #D9D9D9 -->
        <div class="rect-block--third"></div>

        <!-- Fourth block
             position: (595, 189), size: (69 x 69), radius: 50, fill: #D9D9D9 -->
        <div class="rect-block--fourth"></div>

        <!-- Fifth block
             position: (780, 189), size: (69 x 69), radius: 50, fill: #D9D9D9 -->
        <div class="rect-block--fifth"></div>

        <!-- Sixth block
             position: (965, 189), size: (69 x 69), radius: 50, fill: #D9D9D9 -->
        <div class="rect-block--sixth"></div>

        <!-- Seventh block
             position: (1150, 189), size: (69 x 69), radius: 50, fill: #D9D9D9 -->
        <div class="rect-block--seventh"></div>

        <!-- Eighth block
             position: (1335, 189), size: (69 x 69), radius: 50, fill: #D9D9D9 -->
        <div class="rect-block--eighth"></div>

        <!-- Ninth block
             position: (1520, 189), size: (69 x 69), radius: 50, fill: #EDE9E9 -->
        <div class="rect-block--ninth"></div>

        <!-- Tenth block
             position: (1770, 189), size: (69 x 69), radius: 50, fill: #EDE9E9 -->
        <div class="rect-block--tenth"></div>

        <!-- Label text under rectangle: '신제품'
             position: (40, 266), size: (69 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label">
          <span class="rect-label__inner">신제품</span>
        </div>

        <!-- Text under second block: '겨울 이벤트'
             position: (163, 266), size: (192 x 60)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--event">
          <span class="rect-label__inner rect-label__inner--event">
            겨울 이벤트
          </span>
        </div>

        <!-- Text under third block: '수납 가구'
             position: (395, 266), size: (100 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--storage">
          <span class="rect-label__inner">수납 가구</span>
        </div>

        <!-- Text under fourth block: '침대 / 매트리스'
             position: (734, 266), size: (161 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--bed">
          <span class="rect-label__inner">침대 / 매트리스</span>
        </div>

        <!-- Text between third and fourth blocks: '수납 용품'
             position: (578, 266), size: (103 x 41)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--storage-items">
          <span class="rect-label__inner">수납 용품</span>
        </div>

        <!-- Text under sixth block: '소파 / 암체어'
             position: (931, 266), size: (138 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--sofa">
          <span class="rect-label__inner">소파 / 암체어</span>
        </div>

        <!-- Text under seventh block: '식탁/테이블/의자'
             position: (1096, 266), size: (178 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--dining">
          <span class="rect-label__inner">식탁/테이블/의자</span>
        </div>

        <!-- Text under eighth block: '책상/사무용의자'
             position: (1285, 266), size: (170 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--desk">
          <span class="rect-label__inner">책상/사무용의자</span>
        </div>

        <!-- Text under ninth block: '주방가구'
             position: (1509, 266), size: (92 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--kitchen">
          <span class="rect-label__inner">주방가구</span>
        </div>

        <!-- Text under tenth block: '주방 용품'
             position: (1694, 266), size: (92 x 30)
             font: Inter, Thin, 25px, #000000 -->
        <div class="rect-label rect-label--kitchen-items">
          <span class="rect-label__inner">주방 용품</span>
        </div>

        <!-- Vertical line between category icons (1)
             Figma line: position (137, 189), length 107, rotation -90°, stroke #EDE9E9 -->
        <div class="category-line"></div>

        <!-- Vertical line between category icons (2)
             Figma line: position (374, 189), length 107, rotation -90°, stroke #EDE9E9 -->
        <div class="category-line category-line--2"></div>

        <!-- Vertical line between category icons (3)
             Figma line: position (537, 189), length 107, rotation 90°, stroke #EDE9E9 -->
        <div class="category-line category-line--3"></div>

        <!-- Vertical line between category icons (4)
             Figma line: position (710, 189), length 107, rotation 90°, stroke #EDE9E9 -->
        <div class="category-line category-line--4"></div>

        <!-- Vertical line between category icons (5)
             Figma line: position (1082, 189), length 107, rotation -90°, stroke #EDE9E9 -->
        <div class="category-line category-line--5"></div>

        <!-- Vertical line between category icons (6)
             Figma line: position (1280, 189), length 107, rotation 90°, stroke #EDE9E9 -->
        <div class="category-line category-line--6"></div>

        <!-- Vertical line between category icons (7)
             Figma line: position (1476, 189), length 107, rotation 90°, stroke #EDE9E9 -->
        <div class="category-line category-line--7"></div>

        <!-- Vertical line between category icons (8)
             Figma line: position (1649, 189), length 107, rotation 90°, stroke #EDE9E9 -->
        <div class="category-line category-line--8"></div>

        <!-- Vertical line between category icons (9)
             Figma line: position (1829, 182), length 107, rotation 90°, stroke #EDE9E9 -->
        <div class="category-line category-line--9"></div>

        <!-- Vertical line between category icons (10)
             Figma line: position (913, 189), length 107, rotation -90°, stroke #EDE9E9 -->
        <div class="category-line category-line--10"></div>
      </div>
    </section>

    <!-- 'LATENT 겨울 이벤트' heading text (X:38, Y:403, W:353, H:36) -->
    <div class="winter-heading">
      <span class="winter-heading__text">LATENT 겨울 이벤트</span>
    </div>

    <!-- 크리스마스 왼쪽 큰 직사각형 (X:40, Y:468, W:1211, H:1113, radius:15, fill:#EDE9E9) -->
    <div class="winter-rect-left"></div>

    <!-- 크리스마스 오른쪽 큰 직사각형 (X:1221, Y:468, W:671, H:1113, radius:15, fill:#D9D9D9) -->
    <div class="winter-rect-right"></div>

    <!-- '색상별 인테리어' heading text (X:40, Y:1671, W:201, H:36) -->
    <div class="color-heading">
      <span class="color-heading__text">색상별 인테리어</span>
    </div>

    <!-- 첫 번째 색상 카드 - 빨간색 (X:40, Y:1733, W:256, H:337, radius:15, fill:#E25050) -->
    <div class="color-card-red"></div>

    <!-- 두 번째 색상 카드 - 주황색 (X:307, Y:1738, W:256, H:337, radius:15, fill:#F58F00) -->
    <div class="color-card-orange"></div>

    <!-- 주황색 카드 라벨 텍스트 (X:327, Y:1756, W:83, H:36, Inter Bold 30, #000000) -->
    <div class="color-card-orange-label">
      <span class="color-card-orange-label__text">주황색</span>
    </div>

    <!-- 주황색 카드 안 아이콘용 직사각형 + 중앙 화살표 아이콘 -->
    <div class="color-card-orange-icon">
      <svg
        class="color-card-orange-arrow"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="1.5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
        />
      </svg>
    </div>

    <!-- 세 번째 색상 카드 - 노란색 (X:574, Y:1733, W:256, H:337, radius:15, fill:#E4EB19) -->
    <div class="color-card-yellow"></div>

    <!-- 노란색 카드 라벨 텍스트 (X:597, Y:1756, W:83, H:36, Inter Bold 30, #000000) -->
    <div class="color-card-yellow-label">
      <span class="color-card-yellow-label__text">노란색</span>
    </div>

    <!-- 노란색 카드 안 아이콘용 직사각형 + 중앙 화살표 아이콘 (블록 중심: X:606, Y:2020 근처) -->
    <div class="color-card-yellow-icon">
      <svg
        class="color-card-yellow-arrow"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="1.5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
        />
      </svg>
    </div>

    <!-- 네 번째 색상 카드 - 초록색 (X:841, Y:1733, W:256, H:337, radius:15, fill:#12B543) -->
    <div class="color-card-green"></div>

    <!-- 초록색 카드 라벨 텍스트 (X:861, Y:1756, W:83, H:36, Inter Bold 30, #000000) -->
    <div class="color-card-green-label">
      <span class="color-card-green-label__text">초록색</span>
    </div>

    <!-- 초록색 카드 안 아이콘용 직사각형 + 중앙 화살표 아이콘 (블록 중심: X:870, Y:2020 근처) -->
    <div class="color-card-green-icon">
      <svg
        class="color-card-green-arrow"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="1.5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
        />
      </svg>
    </div>

    <!-- 다섯 번째 색상 카드 - 파란색 (X:1105, Y:1733, W:256, H:337, radius:15, fill:#0014F5) -->
    <div class="color-card-blue"></div>

    <!-- 파란색 카드 라벨 텍스트 (X:1125, Y:1756, W:83, H:36, Inter Bold 30, #FFFFFF) -->
    <div class="color-card-blue-label">
      <span class="color-card-blue-label__text">파란색</span>
    </div>

    <!-- 파란색 카드 안 아이콘용 직사각형 + 중앙 화살표 아이콘 (블록 중심: X:1134, Y:2020 근처) -->
    <div class="color-card-blue-icon">
      <svg
        class="color-card-blue-arrow"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="1.5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
        />
      </svg>
    </div>

    <!-- 여섯 번째 색상 카드 - 보라색 (X:1372, Y:1733, W:256, H:337, radius:15, fill:#7632D5) -->
    <div class="color-card-purple"></div>

    <!-- 보라색 카드 라벨 텍스트 (X:1389, Y:1756, W:83, H:36, Inter Bold 30, #000000) -->
    <div class="color-card-purple-label">
      <span class="color-card-purple-label__text">보라색</span>
    </div>

    <!-- 보라색 카드 안 아이콘용 원 + 블록 정중앙 화살표 아이콘 -->
    <div class="color-card-purple-icon">
      <svg
        class="color-card-purple-arrow"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="1.5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
        />
      </svg>
    </div>

    <!-- 일곱 번째 색상 카드 - 검은색 (X:1636, Y:1733, W:256, H:337, radius:15, fill:#000000) -->
    <div class="color-card-black"></div>

    <!-- 검은색 카드 라벨 텍스트 '더 알아보기' (X:1653, Y:1756, W:145, H:36, Inter Bold 30, #FFFFFF) -->
    <div class="color-card-black-label">
      <span class="color-card-black-label__text">더 알아보기</span>
    </div>

    <!-- 검은색 카드 안 아이콘용 원 + 블록 정중앙 화살표 아이콘 -->
    <div class="color-card-black-icon">
      <svg
        class="color-card-black-arrow"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="1.5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
        />
      </svg>
    </div>

    <!-- '겨울 특가 상품 구경하기' 텍스트 (X:40, Y:2233, W:297, H:36, Inter Bold 30, #000000) -->
    <div class="winter-special-heading">
      <span class="winter-special-heading__text">겨울 특가 상품 구경하기</span>
    </div>

    <!-- 겨울 특가 상품 카드 배경 직사각형 (X:40, Y:2305, W:240, H:305, fill:#D9D9D9) -->
    <div class="winter-special-card"></div>

    <!-- 빨간색 카드 라벨 텍스트 (X:57, Y:1756, W:83, H:36, Inter Bold 30, #000000) -->
    <div class="color-card-red-label">
      <span class="color-card-red-label__text">빨간색</span>
    </div>

    <!-- 빨간색 카드 안 아이콘용 직사각형 + 중앙 화살표 아이콘 -->
    <div class="color-card-red-icon">
      <svg
        class="color-card-red-arrow"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="1.5"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
        />
      </svg>
    </div>
  </div>
</template>

<style scoped>
.page {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #ffffff;
}

.top-text {
  position: absolute;
  left: 38px;
  top: 29px;
  width: 131px;
  height: 69px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.top-text__inner {
  font-family: 'Space Mono', monospace;
  font-weight: 400;
  font-size: 35px;
  line-height: 1; /* Figma 기본에 최대한 가깝게 */
  color: #000000;

  /* Stroke: outside 1px (#000000) approximate using text-stroke (WebKit)
     and text-shadow fallback for broader support */
  -webkit-text-stroke-width: 1px;
  -webkit-text-stroke-color: #000000;
  text-shadow:
    -1px 0 0 #000000,
    1px 0 0 #000000,
    0 -1px 0 #000000,
    0 1px 0 #000000;
}

.all-products-pill {
  position: absolute;
  left: 1047px;
  top: 29px;
  width: 128px;
  height: 69px;
  background-color: #ede9e9;
  border-radius: 50px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding-right: 16px;
  gap: 6px;
}

.all-products-pill__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 16px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.all-products-pill__icon {
  width: 18px;
  height: 18px;
  color: #000000;
}

.all-products-bar {
  position: absolute;
  left: 1047px;
  top: 29px;
  width: 542px;
  height: 69px;
  background-color: #d9d9d9;
  border-radius: 50px;
}

.search-placeholder {
  position: absolute;
  left: 1190px;
  top: 29px;
  width: 180px;
  height: 69px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 8px;
  z-index: 2;
  cursor: text;
}

.search-placeholder__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #a39191;
  white-space: nowrap;
}

.search-placeholder__icon {
  width: 24px;
  height: 24px;
  color: #5b4a4a;
}

.search-input {
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  background-color: transparent;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
}

.top-rect {
  position: absolute;
  left: 1622px;
  top: 41px;
  width: 118px;
  height: 45px;
  background-color: #ffffff;
  border: 2px solid #000000;
  border-radius: 50px;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
}

.top-rect__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 20px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.top-rect--black {
  position: absolute;
  left: 1774px;
  top: 41px;
  width: 118px;
  height: 45px;
  background-color: #000000;
  border: 1px solid #000000;
  border-radius: 50px;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
}

.top-rect--black__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 20px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #ffffff;
  white-space: nowrap;
}


.winter-heading {
  position: absolute;
  left: 38px;
  top: 403px;
  width: 353px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.winter-heading__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.winter-rect-left {
  position: absolute;
  left: 40px;
  top: 468px;
  width: 1211px;
  height: 1113px;
  border-radius: 15px;
  background-color: #ede9e9;
}

.winter-rect-right {
  position: absolute;
  left: 1221px;
  top: 468px;
  width: 671px;
  height: 1113px;
  border-radius: 15px;
  background-color: #d9d9d9;
}

.color-heading {
  position: absolute;
  left: 40px;
  top: 1671px;
  width: 201px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-heading__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.winter-special-heading {
  position: absolute;
  left: 40px;
  top: 2233px;
  width: 297px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.winter-special-heading__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.winter-special-card {
  position: absolute;
  left: 40px;
  top: 2305px;
  width: 240px;
  height: 305px;
  background-color: #d9d9d9;
}

.color-card-red {
  position: absolute;
  left: 40px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #e25050;
}

.color-card-red-label {
  position: absolute;
  left: 57px;
  top: 1756px;
  width: 83px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-card-red-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.color-card-red-icon {
  position: absolute;
  left: 57px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-card-red-arrow {
  width: 24px;
  height: 24px;
  color: #000000;
}

.color-card-orange {
  position: absolute;
  left: 307px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #f58f00;
}

.color-card-orange-icon {
  position: absolute;
  left: 327px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-card-orange-arrow {
  width: 24px;
  height: 24px;
  color: #000000;
}

.color-card-yellow {
  position: absolute;
  left: 574px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #e4eb19;
}

.color-card-yellow-label {
  position: absolute;
  left: 597px;
  top: 1756px;
  width: 83px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-card-yellow-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.color-card-yellow-icon {
  position: absolute;
  left: 597px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-card-green {
  position: absolute;
  left: 841px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #12b543;
}

.color-card-green-label {
  position: absolute;
  left: 861px;
  top: 1756px;
  width: 83px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-card-green-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.color-card-green-icon {
  position: absolute;
  left: 861px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-card-green-arrow {
  width: 24px;
  height: 24px;
  color: #000000;
}

.color-card-blue {
  position: absolute;
  left: 1105px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #0014f5;
}

.color-card-blue-icon {
  position: absolute;
  left: 1125px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-card-blue-arrow {
  width: 24px;
  height: 24px;
  color: #000000;
}

.color-card-blue-label {
  position: absolute;
  left: 1125px;
  top: 1756px;
  width: 83px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-card-blue-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #ffffff;
  white-space: nowrap;
}

.color-card-purple {
  position: absolute;
  left: 1372px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #7632d5;
}

.color-card-purple-icon {
  position: absolute;
  left: 1389px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-card-purple-arrow {
  width: 24px;
  height: 24px;
  color: #000000;
}

.color-card-purple-label {
  position: absolute;
  left: 1389px;
  top: 1756px;
  width: 83px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-card-purple-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.color-card-black {
  position: absolute;
  left: 1636px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #000000;
}

.color-card-black-label {
  position: absolute;
  left: 1653px;
  top: 1756px;
  width: 145px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-card-black-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #ffffff;
  white-space: nowrap;
}

.color-card-black-icon {
  position: absolute;
  left: 1653px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.color-card-black-arrow {
  width: 24px;
  height: 24px;
  color: #000000;
}

.color-card-blue {
  position: absolute;
  left: 1105px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #0014f5;
}

.color-card-blue-icon {
  position: absolute;
  left: 1125px;
  top: 1997px;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.color-card-blue {
  position: absolute;
  left: 1105px;
  top: 1733px;
  width: 256px;
  height: 337px;
  border-radius: 15px;
  background-color: #0014f5;
}

.color-card-yellow-arrow {
  width: 24px;
  height: 24px;
  color: #000000;
}

.color-card-orange-label {
  position: absolute;
  left: 327px;
  top: 1756px;
  width: 83px;
  height: 36px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.color-card-orange-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.single-category-icon {
  position: absolute;
  left: 40px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50%;
  background-color: #d9d9d9;
}

.second-category-icon {
  position: absolute;
  left: 177px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50%;
  background-color: #d9d9d9;
}

.single-category-line {
  position: absolute;
  left: 137px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.second-category-line {
  position: absolute;
  left: 292px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.third-category-icon {
  position: absolute;
  left: 331px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 59px;
  background-color: #d9d9d9;
}

.third-category-line {
  position: absolute;
  left: 441px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.fourth-category-icon {
  position: absolute;
  left: 482px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.fourth-category-line {
  position: absolute;
  left: 594px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.fifth-category-icon {
  position: absolute;
  left: 656px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.fifth-category-line {
  position: absolute;
  left: 789px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.sixth-category-icon {
  position: absolute;
  left: 853px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.sixth-category-line {
  position: absolute;
  left: 988px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.seventh-category-icon {
  position: absolute;
  left: 1061px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.seventh-category-line {
  position: absolute;
  left: 1206px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.eighth-category-icon {
  position: absolute;
  left: 1270px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.eighth-category-line {
  position: absolute;
  left: 1408px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.ninth-category-icon {
  position: absolute;
  left: 1454px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.ninth-category-line {
  position: absolute;
  left: 1573px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.tenth-category-icon {
  position: absolute;
  left: 1612px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #ede9e9;
}

.tenth-category-line {
  position: absolute;
  left: 1719px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.single-category-label {
  position: absolute;
  left: 36px;
  top: 266px;
  width: 69px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.single-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.second-category-label {
  position: absolute;
  left: 153px;
  top: 266px;
  width: 123px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.second-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.third-category-label {
  position: absolute;
  left: 316px;
  top: 266px;
  width: 100px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.third-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.fourth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.fourth-category-label {
  position: absolute;
  left: 464px;
  top: 266px;
  width: 100px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.fifth-category-label {
  position: absolute;
  left: 610px;
  top: 266px;
  width: 161px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.fifth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.sixth-category-label {
  position: absolute;
  left: 819px;
  top: 266px;
  width: 138px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.sixth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.seventh-category-label {
  position: absolute;
  left: 1005px;
  top: 266px;
  width: 178px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.seventh-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.eighth-category-label {
  position: absolute;
  left: 1214px;
  top: 266px;
  width: 170px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.eighth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.ninth-category-label {
  position: absolute;
  left: 1443px;
  top: 266px;
  width: 92px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.ninth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.tenth-category-label {
  position: absolute;
  left: 1592px;
  top: 266px;
  width: 92px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.tenth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}


.category-section {
  position: relative;
  width: 100%;
  height: 320px; /* 카테고리 아이콘 + 텍스트 높이 영역 */
  overflow-x: auto;
  overflow-y: visible;
  display: none; /* 카테고리 아이콘/텍스트 전체 숨김 */
}

.category-scroll {
  position: relative; /* 자식 absolute 기준 */
  min-width: 100%; /* 화면 너비에 맞게, 내부에서만 가로 스크롤 */
  height: 100%;
}

.nav-wrapper {
  position: absolute;
  left: 40px; /* 그리드의 좌측 정렬선 */
  top: 120px; /* Figma Y 값 */
  width: 902px; /* 그리드 전체 폭 (더보기 포함) */
  display: flex;
  flex-direction: column;
}

.nav-menu {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between; /* 첫 번째와 '더보기'를 좌우 정렬선에 맞춤 */
  width: 100%;
}

.nav-item {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap; /* 줄바꿈 금지 */
}

.nav-item--label {
  /* 필요하면 나중에 굵기/색상 차이 줄 수 있음 */
}

.nav-divider {
  margin-top: 15px; /* 텍스트 메뉴와 벡터 라인 사이 고정 간격 (사용자 조정) */
  width: 100%;
  height: 1px; /* Figma 높이 0인 선을 1px로 표현 */

  /* Fill & stroke 근사 */
  background-color: #d9d9d9;

  /* 안쪽 그림자 효과 근사 */
  box-shadow: inset 0 1px 0 rgba(0, 0, 0, 0.06);
}

.nav-divider__stroke {
  width: 100%;
  height: 1px;
}

.nav-divider__stroke line {
  stroke: #ffffff;
  stroke-width: 1;
}

.rect-block {
  position: absolute;
  left: 40px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

/* Second icon block: Figma '벡터 경로'
   position: (225, 189), size: (69 x 69), fill: #D9D9D9 */
.rect-block--vector {
  position: absolute;
  left: 225px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.rect-block--third {
  position: absolute;
  left: 410px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 59px;
  background-color: #d9d9d9;
}

.rect-block--fourth {
  position: absolute;
  left: 595px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.rect-block--fifth {
  position: absolute;
  left: 780px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.rect-block--sixth {
  position: absolute;
  left: 965px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.rect-block--seventh {
  position: absolute;
  left: 1150px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.rect-block--eighth {
  position: absolute;
  left: 1335px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #d9d9d9;
}

.rect-block--ninth {
  position: absolute;
  left: 1520px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #ede9e9;
}

.rect-block--tenth {
  position: absolute;
  left: 1770px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #ede9e9;
}

.rect-label {
  position: absolute;
  left: 40px;
  top: 266px;
  width: 69px;
  height: 30px;
  display: flex;
  align-items: flex-end; /* 텍스트를 항상 하단 기준으로 정렬 */
  justify-content: flex-start;
}

.rect-label__inner {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.fifth-category-label {
  position: absolute;
  left: 610px;
  top: 266px;
  width: 161px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.fifth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.sixth-category-label {
  position: absolute;
  left: 819px;
  top: 266px;
  width: 138px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.sixth-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.seventh-category-label {
  position: absolute;
  left: 986.5px;
  top: 266px;
  width: 178px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.seventh-category-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.rect-label__inner--event {
  /* '크리스마스 이벤트'는 항상 한 줄 표시 */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis; /* 폭이 부족하면 말줄임표 처리 */
}

.rect-label--event {
  left: 163px;
  top: 266px;
  width: 192px;
  height: 30px;
}

.rect-label--storage {
  left: 395px;
  top: 266px;
  width: 100px;
  height: 30px;
}

.rect-label--bed {
  left: 734px;
  top: 266px;
  width: 161px;
  height: 30px;
}

.rect-label--storage-items {
  left: 578px;
  top: 266px;
  width: 103px;
  height: 30px; /* 다른 카테고리와 높이 통일해서 베이스라인 맞춤 */
}

.rect-label--sofa {
  left: 931px;
  top: 266px;
  width: 138px;
  height: 30px;
}

.rect-label--dining {
  left: 1087px;
  top: 266px;
  width: 178px;
  height: 30px;
}

.rect-label--desk {
  left: 1285px;
  top: 266px;
  width: 170px;
  height: 30px;
}

.rect-label--kitchen {
  left: 1509px;
  top: 266px;
  width: 92px;
  height: 30px;
}

.rect-label--kitchen-items {
  left: 1694px;
  top: 266px;
  width: 92px;
  height: 30px;
}

.category-line {
  position: absolute;
  left: 137px;
  top: 189px;
  width: 1px;
  height: 107px;
  background-color: #ede9e9;
}

.category-line--2 {
  left: 374px;
}

.category-line--3 {
  left: 537px;
}

.category-line--4 {
  left: 710px;
}

.category-line--5 {
  left: 1082px;
}

.category-line--6 {
  left: 1280px;
}

.category-line--7 {
  left: 1476px;
}

.category-line--8 {
  left: 1649px;
}

.category-line--9 {
  left: 1829px;
  top: 182px;
}

.category-line--10 {
  left: 913px;
}
</style>
