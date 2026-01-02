<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

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

const isAllProductsOpen = ref(false)
const allProductsItems = [
  '베스트',
  '제작 상품 보기',
  '인테리어 디자인 클래스',
  '전문가 찾기',
  '이벤트',
  '커뮤니티',
  '문의'
]

const toggleAllProducts = () => {
  isAllProductsOpen.value = !isAllProductsOpen.value
}

const bottomFooterScrollRef = ref<HTMLElement | null>(null)
const bottomFinalScrollRef = ref<HTMLElement | null>(null)
const bottomInteriorScrollRef = ref<HTMLElement | null>(null)
const livingRoomScrollRef = ref<HTMLElement | null>(null)
const kitchenScrollRef = ref<HTMLElement | null>(null)
const bedroomScrollRef = ref<HTMLElement | null>(null)
const porchScrollRef = ref<HTMLElement | null>(null)
const balconyScrollRef = ref<HTMLElement | null>(null)
const toiletScrollRef = ref<HTMLElement | null>(null)
const dressingroomScrollRef = ref<HTMLElement | null>(null)
const bottomLastScrollRef = ref<HTMLElement | null>(null)

const isBottomFooterHovered = ref(false)
const isBottomFinalHovered = ref(false)
const isBottomInteriorHovered = ref(false)

// 인테리어 섹션: 최근 사용자 조작 시점 (ms)
const bottomInteriorLastInteraction = ref(0)
const BOTTOM_INTERIOR_PAUSE_MS = 1800

let bottomFooterAutoScrollId: number | null = null
let bottomFinalAutoScrollId: number | null = null
let bottomInteriorAutoScrollId: number | null = null
let bottomInteriorDebugLogged = false

const scrollHorizontally = (event: WheelEvent, el: HTMLElement | null) => {
  if (!el) return

  // 세로/가로 입력 중 더 큰 값을 가로 스크롤로 사용
  const delta =
    Math.abs(event.deltaY) > Math.abs(event.deltaX)
      ? event.deltaY
      : event.deltaX

  el.scrollLeft += delta
}

const handleBottomFooterWheel = (event: WheelEvent) => {
  scrollHorizontally(event, bottomFooterScrollRef.value)
}

const handleBottomFinalWheel = (event: WheelEvent) => {
  scrollHorizontally(event, bottomFinalScrollRef.value)
}

const markBottomInteriorInteraction = () => {
  bottomInteriorLastInteraction.value = performance.now()
}

const handleBottomInteriorWheel = (event: WheelEvent) => {
  markBottomInteriorInteraction()
  scrollHorizontally(event, bottomInteriorScrollRef.value)
}

const handleLivingRoomWheel = (event: WheelEvent) => {
  scrollHorizontally(event, livingRoomScrollRef.value)
}

const handleKitchenWheel = (event: WheelEvent) => {
  scrollHorizontally(event, kitchenScrollRef.value)
}

const handleBedroomWheel = (event: WheelEvent) => {
  scrollHorizontally(event, bedroomScrollRef.value)
}

const handlePorchWheel = (event: WheelEvent) => {
  scrollHorizontally(event, porchScrollRef.value)
}

const handleBalconyWheel = (event: WheelEvent) => {
  scrollHorizontally(event, balconyScrollRef.value)
}

const handleToiletWheel = (event: WheelEvent) => {
  scrollHorizontally(event, toiletScrollRef.value)
}

const handleDressingroomWheel = (event: WheelEvent) => {
  scrollHorizontally(event, dressingroomScrollRef.value)
}

const handleBottomLastWheel = (event: WheelEvent) => {
  scrollHorizontally(event, bottomLastScrollRef.value)
}

const handleBottomFooterMouseEnter = () => {
  isBottomFooterHovered.value = true
}

const handleBottomFooterMouseLeave = () => {
  isBottomFooterHovered.value = false
}

const handleBottomFinalMouseEnter = () => {
  isBottomFinalHovered.value = true
}

const handleBottomFinalMouseLeave = () => {
  isBottomFinalHovered.value = false
}

const handleBottomInteriorMouseEnter = () => {
  isBottomInteriorHovered.value = true
}

const handleBottomInteriorMouseLeave = () => {
  isBottomInteriorHovered.value = false
}

const startAutoScroll = (
  getEl: () => HTMLElement | null,
  getIsHovered: () => boolean,
  setId: (id: number | null) => void,
  speed = 0.3
) => {
  const existingId = ((): number | null => {
    // dummy accessor; 실제 값은 setId를 통해 관리
    return null
  })()

  if (existingId !== null) {
    cancelAnimationFrame(existingId)
  }

  const step = () => {
    const el = getEl()
    if (el && !getIsHovered()) {
      const maxScroll = el.scrollWidth - el.clientWidth
      if (maxScroll > 0) {
        if (el.scrollLeft >= maxScroll - 1) {
          el.scrollLeft = 0
        } else {
          el.scrollLeft += speed
        }
      }
    }
    const id = requestAnimationFrame(step)
    setId(id)
  }

  const id = requestAnimationFrame(step)
  setId(id)
}

const startBottomInteriorAutoScroll = () => {
  // 기존 인테리어 섹션 자동 스크롤 루프 정리
  stopAutoScroll(bottomInteriorAutoScrollId, id => (bottomInteriorAutoScrollId = id))

  const step = () => {
    const el = bottomInteriorScrollRef.value

    if (el && !isBottomInteriorHovered.value) {
      const now = performance.now()
      // 최근 사용자 조작 이후 일정 시간 동안은 자동 스크롤 일시 정지
      if (now - bottomInteriorLastInteraction.value > BOTTOM_INTERIOR_PAUSE_MS) {
        const scrollWidth = el.scrollWidth
        const clientWidth = el.clientWidth
        const maxScroll = scrollWidth - clientWidth

        // 한 번만 디버그 로그 출력해서 실제 스크롤 가능 여부 확인
        if (!bottomInteriorDebugLogged) {
          // eslint-disable-next-line no-console
          console.log('[bottom-interior-scroll]', {
            scrollWidth,
            clientWidth,
            maxScroll,
            scrollLeft: el.scrollLeft,
          })
          bottomInteriorDebugLogged = true
        }

        // 카드 폭/개수/overflow 설정이 충분해서 실제로 가로 오버플로우가 있을 때만 자동 스크롤
        if (maxScroll > 0) {
          // 카드를 두 번 배치한 트랙을 부드럽게 순환하기 위해,
          // 전체 scrollWidth의 절반을 기준으로 루프를 돌린다.
          const loopWidth = scrollWidth / 2
          const speed = 0.3 // 프리미엄 톤을 위한 매우 느린 속도

          el.scrollLeft += speed

          // 절반 지점을 넘어서면 loopWidth 만큼 되돌려 자연스럽게 반복
          if (el.scrollLeft >= loopWidth) {
            el.scrollLeft -= loopWidth
          }
        }
      }
    }

    const id = requestAnimationFrame(step)
    bottomInteriorAutoScrollId = id
  }

  bottomInteriorAutoScrollId = requestAnimationFrame(step)
}

const stopAutoScroll = (id: number | null, setId: (id: number | null) => void) => {
  if (id !== null) {
    cancelAnimationFrame(id)
    setId(null)
  }
}

onMounted(() => {
  startAutoScroll(
    () => bottomFinalScrollRef.value,
    () => isBottomFinalHovered.value,
    id => (bottomFinalAutoScrollId = id),
    0.25
  )

  // 인테리어 섹션은 사용자 조작 기반 일시 정지를 위해 별도 루프 사용
  startBottomInteriorAutoScroll()
})

onUnmounted(() => {
  stopAutoScroll(bottomFooterAutoScrollId, id => (bottomFooterAutoScrollId = id))
  stopAutoScroll(bottomFinalAutoScrollId, id => (bottomFinalAutoScrollId = id))
  stopAutoScroll(bottomInteriorAutoScrollId, id => (bottomInteriorAutoScrollId = id))
})
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

    <!-- 상단 '모든 제품' 강조 직사각형 + 드롭다운 -->
    <div class="all-products-wrapper">
      <button class="all-products-pill" type="button" @click="toggleAllProducts">
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
      </button>

      <transition name="all-products-dropdown">
        <ul
          v-if="isAllProductsOpen"
          class="all-products-dropdown"
        >
          <li
            v-for="item in allProductsItems"
            :key="item"
            class="all-products-dropdown__item"
          >
            {{ item }}
          </li>
        </ul>
      </transition>
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

  <!-- Category items: icon + label (원 + 텍스트를 하나의 인터랙션 단위로 묶음) -->
  <div class="category-item category-item--1" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">신제품</span>
  </div>

  <div class="category-item category-item--2" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">겨울 이벤트</span>
  </div>

  <div class="category-item category-item--3" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">수납 가구</span>
  </div>

  <div class="category-item category-item--4" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">수납 용품</span>
  </div>

  <div class="category-item category-item--5" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">침대 / 매트리스</span>
  </div>

  <div class="category-item category-item--6" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">소파 / 암체어</span>
  </div>

  <div class="category-item category-item--7" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">식탁 / 테이블 / 의자</span>
  </div>

  <div class="category-item category-item--8" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">책상/사무용의자</span>
  </div>

  <div class="category-item category-item--9" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">주방가구</span>
  </div>

  <div class="category-item category-item--10" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">주방 용품</span>
  </div>

  <div class="category-item category-item--11" tabindex="0">
    <div class="category-item__icon"></div>
    <span class="category-item__label">조명</span>
  </div>

  <!-- Separator line next to the first icon (X:137, Y:189, H:107, #EDE9E9) -->
  <div class="single-category-line"></div>

  <!-- Separator line between first and second icons (X:292, Y:189, H:107, #EDE9E9) -->
  <div class="second-category-line"></div>

  <!-- Separator line to the right of third icon (X:441, Y:189, H:107, #EDE9E9) -->
  <div class="third-category-line"></div>

  <!-- Separator line to the right of fourth icon (X:594, Y:189, H:107, #EDE9E9) -->
  <div class="fourth-category-line"></div>

  <!-- Separator line to the right of fifth icon (X:789, Y:189, H:107, #EDE9E9) -->
  <div class="fifth-category-line"></div>

  <!-- Separator line to the right of sixth icon (X:988, Y:189, H:107, #EDE9E9) -->
  <div class="sixth-category-line"></div>

  <!-- Separator line to the right of seventh icon (X:1206, Y:189, H:107, #EDE9E9) -->
  <div class="seventh-category-line"></div>

  <!-- Separator line to the right of eighth icon (X:1408, Y:189, H:107, #EDE9E9) -->
  <div class="eighth-category-line"></div>

  <!-- Separator line to the right of ninth icon (X:1573, Y:189, H:107, #EDE9E9) -->
  <div class="ninth-category-line"></div>

  <!-- Separator line to the right of tenth icon (X:1719, Y:189, H:107, #EDE9E9) -->
  <div class="tenth-category-line"></div>

  <!-- 기존 개별 텍스트/아이콘 div는 category-item 안으로 통합됨 -->

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

    <!-- 두 번째 겨울 특가 상품 카드 배경 직사각형 (X:296, Y:2305, W:240, H:305, fill:#D9D9D9) -->
    <div class="winter-special-card winter-special-card--second"></div>

    <!-- 세 번째 겨울 특가 상품 카드 배경 직사각형 (X:552, Y:2305, W:240, H:305, fill:#D9D9D9) -->
    <div class="winter-special-card winter-special-card--third"></div>

    <!-- 네 번째 겨울 특가 상품 카드 배경 직사각형 (X:808, Y:2305, W:240, H:305, fill:#D9D9D9) -->
    <div class="winter-special-card winter-special-card--fourth"></div>

    <!-- 다섯 번째 겨울 특가 상품 카드 배경 직사각형 (X:1064, Y:2305, W:240, H:305, fill:#D9D9D9) -->
    <div class="winter-special-card winter-special-card--fifth"></div>

    <!-- 여섯 번째 겨울 특가 상품 카드 배경 직사각형 (X:1320, Y:2305, W:240, H:305, fill:#D9D9D9) -->
    <div class="winter-special-card winter-special-card--sixth"></div>

    <!-- 일곱 번째 겨울 특가 상품 카드 배경 직사각형 (X:1576, Y:2305, W:240, H:305, fill:#D9D9D9) -->
    <div class="winter-special-card winter-special-card--seventh"></div>

    <!-- 겨울 특가 카드 안 텍스트 블록 (X:40, Y:2620, W:240, H:130) -->
    <div class="winter-special-card-text">
      <span class="winter-special-card-text__brand">LATENT</span>
      <span class="winter-special-card-text__name">펜던트 램프</span>
      <span class="winter-special-card-text__price">160$</span>
      <span class="winter-special-card-text__original">정가 : 170$</span>
    </div>

    <!-- 두 번째 겨울 특가 카드 안 텍스트 블록 (X:298, Y:2620, W:207, H:142) -->
    <div class="winter-special-card-text winter-special-card-text--second">
      <span class="winter-special-card-text__brand">LATENT</span>
      <span class="winter-special-card-text__name"
        >검정 가죽 패딩 브라운 나무 의자</span
      >
      <span class="winter-special-card-text__price">102$</span>
      <span class="winter-special-card-text__original">정가 : 113$</span>
    </div>

    <!-- 세 번째 겨울 특가 카드 안 텍스트 블록 (X:552, Y:2620, W:220, H:142) -->
    <div class="winter-special-card-text winter-special-card-text--third">
      <span class="winter-special-card-text__brand">LATENT</span>
      <span class="winter-special-card-text__name"
        >내추럴 코튼 퀄팅 헤드보드 침대</span
      >
      <span class="winter-special-card-text__price">263$</span>
      <span class="winter-special-card-text__original">정가 : 270$</span>
    </div>

    <!-- 네 번째 겨울 특가 카드 안 텍스트 블록 (X:808, Y:2620, W:240, H:142) -->
    <div class="winter-special-card-text winter-special-card-text--fourth">
      <span class="winter-special-card-text__brand">LATENT</span>
      <span class="winter-special-card-text__name"
        >클래식 로마 숫자 앤틱 탁상시계</span
      >
      <span class="winter-special-card-text__price">22.6$</span>
      <span class="winter-special-card-text__original">정가 : 25$</span>
    </div>
    <!-- 다섯 번째 겨울 특가 카드 안 텍스트 블록 (X:1064, Y:2620, W:240, H:142) -->
    <div class="winter-special-card-text winter-special-card-text--fifth">
      <span class="winter-special-card-text__brand">LATENT</span>
      <span class="winter-special-card-text__name"
        >파스텔 세라믹 테이블웨어 세트</span
      >
      <span class="winter-special-card-text__price">51$</span>
      <span class="winter-special-card-text__original">정가 : 60$</span>
    </div>
    <!-- 여섯 번째 겨울 특가 카드 안 텍스트 블록 (X:1576, Y:2620, W:240, H:132) -->
    <div class="winter-special-card-text winter-special-card-text--sixth">
      <span class="winter-special-card-text__brand">LATENT</span>
      <span class="winter-special-card-text__name"
        >헤리티지 북 인테리어 소품</span
      >
      <span class="winter-special-card-text__price">19.35$</span>
      <span class="winter-special-card-text__original">정가 : 20$</span>
    </div>
    <!-- 일곱 번째 겨울 특가 카드 안 텍스트 블록 (X:1576, Y:2620, W:240, H:132) -->
    <div class="winter-special-card-text winter-special-card-text--seventh">
      <span class="winter-special-card-text__brand">LATENT</span>
      <span class="winter-special-card-text__name"
        >빈티지 캔버스 런치박스</span
      >
      <span class="winter-special-card-text__price">8.22$</span>
      <span class="winter-special-card-text__original">정가 : 10$</span>
    </div>

    <!-- 겨울 특가 영역 오른쪽 하단 원형 블록 + 화살표 아이콘 (X:1840, Y:2433, W:60, H:60, fill:#000000) -->
    <div class="winter-special-circle">
      <svg
        class="winter-special-circle__icon"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="m8.25 4.5 7.5 7.5-7.5 7.5"
        />
      </svg>
    </div>

    <!-- 하단 대형 배너 직사각형 (X:38, Y:2842, W:1206, H:633, radius:15, fill:#B8B4AE) -->
    <div class="new-products-panel"></div>

    <!-- 하단 우측 배너 직사각형 (X:1215, Y:2842, W:601, H:633, radius:15, fill:#D9D9D9) -->
    <div class="new-products-panel new-products-panel--right"></div>

    <!-- 'LATENT 멤버십, 더 나은 공간의 시작' 텍스트 (X:75, Y:2889, W:774, H:61, Inter Bold 50) -->
    <div class="membership-heading">
      <span class="membership-heading__text">LATENT 멤버십, 더 나은 공간의 시작</span>
    </div>

    <!-- 멤버십 설명 본문 텍스트 -->
    <div class="membership-body">
      <p class="membership-body__line">
        공간을 더 가치 있게, LATENT 멤버십으로 완성하세요.
      </p>
      <p class="membership-body__line">
        회원 전용 가격과 큐레이션으로 일상 속 프리미엄 가구를 부담 없이
        만나보세요.
      </p>
    </div>

    <!-- 하단 LATENT 텍스트 (X:75, Y:3370, W:276, H:85, Inter Bold 70) -->
    <div class="membership-logo-text">
      <span class="membership-logo-text__inner">LATENT</span>
    </div>

    <!-- 우측 배너 '멤버십 혜택' 제목 텍스트 (X:1255, Y:2884, W:218, H:54, Inter Bold 45) -->
    <div class="membership-benefit-heading">
      <span class="membership-benefit-heading__text">멤버십 혜택</span>
    </div>

    <!-- 우측 배너 멤버십 혜택 리스트 -->
    <div class="membership-benefit-list">
      <p class="membership-benefit-list__item">✔ 회원 전용 상시 할인 &amp; 쿠폰</p>
      <p class="membership-benefit-list__item">✔ 신상품·한정 컬렉션 우선 구매</p>
      <p class="membership-benefit-list__item">✔ 프리미엄 배송·설치·AS 케어</p>
    </div>

    <!-- 우측 배너 CTA: 'LATENT 혜택 직접 이용해보기' + 아이콘 -->
    <div class="membership-benefit-cta">
      <div class="membership-benefit-cta__text">
        <span class="membership-benefit-cta__line">LATENT 혜택</span>
        <span class="membership-benefit-cta__line membership-benefit-cta__line--highlight">
          지금 경험해보기
        </span>
      </div>
      <div class="membership-benefit-cta__icon-wrap">
        <svg
          class="membership-benefit-cta__icon"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M8.25 9V5.25A2.25 2.25 0 0 1 10.5 3h6a2.25 2.25 0 0 1 2.25 2.25v13.5A2.25 2.25 0 0 1 16.5 21h-6a2.25 2.25 0 0 1-2.25-2.25V15M12 9l3 3m0 0-3 3m3-3H2.25"
          />
        </svg>
      </div>
    </div>
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

    <!-- 하단 '신제품 보러가기' 텍스트 (X:40, Y:3588, W:202, H:36, Inter Regular 30) -->
    <div class="new-products-link">
      <span class="new-products-link__text">신제품 보러가기</span>
    </div>

    <!-- 하단 추가 영역 배경 벡터 (X:36.5, Y:3649, W:661, H:924, radius:15, fill:#D9D9D9, stroke:#FFFFFF) -->
    <div class="bottom-extra-panel"></div>

    <!-- 하단 우측 추가 영역 배경 벡터 (X:698, Y:3649, W:695, H:414, radius:15, fill:#D9D9D9, stroke:#FFFFFF) -->
    <div class="bottom-extra-panel bottom-extra-panel--right"></div>

    <!-- 하단 우측 아래 추가 영역 배경 벡터 (X:698, Y:4063, W:671, H:510, radius:15, fill:#D9D9D9, stroke:#FFFFFF) -->
    <div class="bottom-extra-panel bottom-extra-panel--right-lower"></div>

    <!-- 하단 가장 오른쪽 세로형 추가 영역 (X:1369, Y:3649, W:447, H:924, radius:15, fill:#D9D9D9, stroke:#FFFFFF) -->
    <div class="bottom-extra-panel bottom-extra-panel--far-right"></div>

    <!-- LATENT 멤버십 회원 전용 할인/쿠폰 문구 (X:725, Y:3973, W:480, H:72)
         첫째 줄 끝을 '제공되는'까지 고정 -->
    <div class="membership-coupon-text">
      <p class="membership-coupon-text__line">
        LATENT 멤버십 회원에게만 제공되는<br />
        전용 할인과 쿠폰 혜택
      </p>
    </div>

    <!-- 신제품 우선 구매 기회 문구 (X:725, Y:4484, W:464, H:72)
         첫째 줄: '신제품을 가장 먼저' / 둘째 줄: '출시 전 우선 구매 기회를 제공합니다' -->
    <div class="membership-early-access-text">
      <p class="membership-early-access-text__line">
        신제품을 가장 먼저<br />
        출시 전 우선 구매 기회를 제공합니다
      </p>
    </div>

    <!-- LATENT 새로운 디자인 설명 문구 (X:1387, Y:4484, W:412, H:72)
         첫째 줄: 'LATENT의 새로운 디자인' / 둘째 줄: '공간에 자연스럽게 스며드는 형태' -->
    <div class="membership-design-text">
      <p class="membership-design-text__line">
        LATENT의 새로운 디자인<br />
        공간에 자연스럽게 스며드는 형태
      </p>
    </div>

    <!-- 하단 패널 '신제품 보러가기' 텍스트 + 아이콘 (X:53, Y:4436, W:202, H:36, Inter Regular 30) -->
    <div class="bottom-new-products-link">
      <span class="bottom-new-products-link__text">신제품 보러가기</span>
      <svg
        class="bottom-new-products-link__icon"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M8.25 9V5.25A2.25 2.25 0 0 1 10.5 3h6a2.25 2.25 0 0 1 2.25 2.25v13.5A2.25 2.25 0 0 1 16.5 21h-6a2.25 2.25 0 0 1-2.25-2.25V15M12 9l3 3m0 0-3 3m3-3H2.25"
        />
      </svg>
    </div>

    <!-- 페이지 맨 아래 전체 배경 직사각형 (X:0, Y:4709, W:1929, H:633, fill:#F3F3F3) -->
    <div class="bottom-footer-bg"></div>

    <!-- 하단 카드 가로 스크롤 영역 (자동 스크롤 + hover 시 일시 정지) -->
    <div
      class="bottom-footer-scroll"
      ref="bottomFooterScrollRef"
      @wheel.prevent="handleBottomFooterWheel"
      @mouseenter="handleBottomFooterMouseEnter"
      @mouseleave="handleBottomFooterMouseLeave"
    >
      <!-- 스크롤 UX를 위해 6개의 카드 생성
           맨 왼쪽 카드(첫 번째)만 내용 표시, 나머지는 비워서 빈 패널로 사용 -->
      <div
        v-for="card in 6"
        :key="card"
        class="bottom-footer-card"
      >
        <template v-if="card === 1">
          <div class="bottom-footer-card__label">
            모든 혜택 보러가기
          </div>
          <button class="bottom-footer-card__icon" type="button">
            <svg
              class="bottom-footer-card__icon-svg"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M8.25 9V5.25A2.25 2.25 0 0 1 10.5 3h6a2.25 2.25 0 0 1 2.25 2.25v13.5A2.25 2.25 0 0 1 16.5 21h-6a2.25 2.25 0 0 1-2.25-2.25V15M12 9l3 3m0 0-3 3m3-3H2.25"
              />
            </svg>
          </button>
        </template>
      </div>
    </div>

    <!-- 최하단 대형 섹션 배경 직사각형 (X:41, Y:5463, W:1845, H:868, radius:15, fill:#D9D9D9) -->
    <div class="bottom-large-panel">
      <button class="bottom-large-panel__play" type="button">
        <svg
          class="bottom-large-panel__play-svg"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M8.25 4.5v15l11.25-7.5-11.25-7.5z"
          />
        </svg>
      </button>
    </div>

    <!-- 페이지 최하단 배경 직사각형 (X:0, Y:6471, W:1929, H:616, fill:#F3F3F3) -->
    <div class="bottom-final-bg"></div>

    <!-- 최하단 카드 가로 스크롤 영역 (자동 스크롤 + hover 시 일시 정지) -->
    <div
      class="bottom-final-scroll"
      ref="bottomFinalScrollRef"
      @wheel.prevent="handleBottomFinalWheel"
      @mouseenter="handleBottomFinalMouseEnter"
      @mouseleave="handleBottomFinalMouseLeave"
    >
      <div
        v-for="card in 6"
        :key="card"
        class="bottom-final-card"
      ></div>
    </div>

    <!-- 'LATENT로 꾸민 인테리어' 텍스트 (X:41, Y:7221, W:326, H:36, Inter Regular 30, #000000) -->
    <div class="bottom-final-heading">
      <span class="bottom-final-heading__text">LATENT로 꾸민 인테리어</span>
    </div>

    <!-- '여러분들의 집을 자랑해보세요!' 보조 텍스트 (X:41, Y:7271, W:256, H:24, Inter Regular 20, #000000) -->
    <div class="bottom-final-subheading">
      <span class="bottom-final-subheading__text">
        여러분들의 집을 자랑해보세요!
      </span>
    </div>

    <!-- 최하단 인테리어 카드용 가로 스크롤 영역 -->
    <div
      class="bottom-interior-scroll"
      ref="bottomInteriorScrollRef"
      @wheel.prevent="handleBottomInteriorWheel"
      @mouseenter="handleBottomInteriorMouseEnter"
      @mouseleave="handleBottomInteriorMouseLeave"
      @mousedown="markBottomInteriorInteraction"
      @touchstart="markBottomInteriorInteraction"
    >
      <div class="bottom-interior-track">
        <!-- 기본 카드 세트 -->
        <div
          v-for="card in 6"
          :key="`set-a-${card}`"
          class="bottom-final-large-card"
        ></div>
        <!-- 끊김 없는 루프를 위한 복제 카드 세트 (aria-hidden) -->
        <div
          v-for="card in 6"
          :key="`set-b-${card}`"
          class="bottom-final-large-card bottom-final-large-card--clone"
          aria-hidden="true"
        ></div>
      </div>
    </div>

    <!-- Living Room 텍스트 (X:41, Y:8355, W:301, H:83, Gotu Regular 50, #000000) -->
    <div class="living-room-heading">
      <span class="living-room-heading__text">Living Room</span>
    </div>

    <!-- Living Room 하단 카드 가로 스크롤 영역 (wheel 세로 입력을 가로 스크롤로 매핑) -->
    <section
      class="living-room-scroll"
      ref="livingRoomScrollRef"
      @wheel.prevent="handleLivingRoomWheel"
    >
      <div
        v-for="card in 10"
        :key="card"
        class="living-room-card"
      ></div>
    </section>

    <!-- Kitchen 텍스트 (X:41, Y:9168, W:188, H:83, Gotu Regular 50, #000000) -->
    <div class="kitchen-heading">
      <span class="kitchen-heading__text">Kitchen</span>
    </div>

    <!-- Kitchen 하단 카드 가로 스크롤 영역 (Living Room 섹션과 동일한 구조) -->
    <section
      class="kitchen-scroll"
      ref="kitchenScrollRef"
      @wheel.prevent="handleKitchenWheel"
    >
      <div
        v-for="card in 10"
        :key="card"
        class="kitchen-card"
      ></div>
    </section>

    <!-- Bedroom 텍스트 (X:41, Y:9936, W:237, H:83, Gotu Regular 50, #000000) -->
    <div class="bedroom-heading">
      <span class="bedroom-heading__text">Bedroom</span>
    </div>

    <!-- Bedroom 하단 카드 가로 스크롤 영역 (Living Room/Kitchen 섹션과 동일한 구조) -->
    <section
      class="bedroom-scroll"
      ref="bedroomScrollRef"
      @wheel.prevent="handleBedroomWheel"
    >
      <div
        v-for="card in 10"
        :key="card"
        class="bedroom-card"
      ></div>
    </section>

    <!-- Porch 텍스트 (X:41, Y:10721, W:144, H:83, Gotu Regular 50, #000000) -->
    <div class="porch-heading">
      <span class="porch-heading__text">Porch</span>
    </div>

    <!-- Porch 하단 카드 가로 스크롤 영역 (위 섹션들과 동일한 구조) -->
    <section
      class="porch-scroll"
      ref="porchScrollRef"
      @wheel.prevent="handlePorchWheel"
    >
      <div
        v-for="card in 10"
        :key="card"
        class="porch-card"
      ></div>
    </section>

    <!-- Balcony 텍스트 (X:41, Y:11503, W:194, H:83, Gotu Regular 50, #000000) -->
    <div class="balcony-heading">
      <span class="balcony-heading__text">Balcony</span>
    </div>

    <!-- Balcony 하단 카드 가로 스크롤 영역 (위 섹션들과 동일한 구조) -->
    <section
      class="balcony-scroll"
      ref="balconyScrollRef"
      @wheel.prevent="handleBalconyWheel"
    >
      <div
        v-for="card in 10"
        :key="card"
        class="balcony-card"
      ></div>
    </section>

    <!-- Toilet 텍스트 (X:41, Y:12287, W:134, H:83, Gotu Regular 50, #000000) -->
    <div class="toilet-heading">
      <span class="toilet-heading__text">Toilet</span>
    </div>

    <!-- Toilet 하단 카드 가로 스크롤 영역 (위 섹션들과 동일한 구조) -->
    <section
      class="toilet-scroll"
      ref="toiletScrollRef"
      @wheel.prevent="handleToiletWheel"
    >
      <div
        v-for="card in 10"
        :key="card"
        class="toilet-card"
      ></div>
    </section>

    <!-- Dressingroom 텍스트 (X:41, Y:13071, W:355, H:83, Gotu Regular 50, #000000) -->
    <div class="dressingroom-heading">
      <span class="dressingroom-heading__text">Dressingroom</span>
    </div>

    <!-- Dressingroom 하단 카드 가로 스크롤 영역 (위 섹션들과 동일한 구조) -->
    <section
      class="dressingroom-scroll"
      ref="dressingroomScrollRef"
      @wheel.prevent="handleDressingroomWheel"
    >
      <div
        v-for="card in 10"
        :key="card"
        class="dressingroom-card"
      ></div>
    </section>

    <!-- 페이지 최하단 추가 배경 직사각형 (X:-2, Y:13901, W:1925, H:671, fill:#F3F3F3) -->
    <div class="bottom-last-bg"></div>

    <!-- 'LATENT에 대해 더 많이 알아보기' 텍스트 (X:39, Y:13938, W:436, H:36, Inter Regular 30, #000000) -->
    <div class="bottom-last-link">
      <span class="bottom-last-link__text">LATENT에 대해 더 많이 알아보기</span>
    </div>

    <!-- 최하단 카드 가로 스크롤 영역 (wheel 세로 입력을 가로 스크롤로 매핑) -->
    <section
      class="bottom-last-scroll"
      ref="bottomLastScrollRef"
      @wheel.prevent="handleBottomLastWheel"
    >
      <div
        v-for="card in 9"
        :key="card"
        class="bottom-last-card"
      ></div>
      <!-- 스크롤 맨 끝 CTA 카드 -->
      <button class="bottom-last-card bottom-last-card--cta" type="button">
        <span class="bottom-last-cta__text">더 많은 혜택 보러가기</span>
        <span class="bottom-last-cta__icon-wrap">
          <svg
            class="bottom-last-cta__icon"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
          >
            <path
              d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </span>
      </button>
    </section>

    <!-- 페이지 최하단 추가 큰 패널 (X:-2, Y:14669, W:1927, H:1055, radius:10, fill:#D9D9D9) -->
    <div class="bottom-extra-bg"></div>
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
  position: relative;
  width: 128px;
  height: 69px;
  background-color: #ede9e9;
  border-radius: 50px;
  border: none;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding-right: 16px;
  gap: 6px;
  outline: none;
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

.all-products-wrapper {
  position: absolute;
  left: 1047px;
  top: 29px;
  height: 69px;
  display: inline-flex;
  align-items: flex-start;
}

.all-products-dropdown {
  position: absolute;
  top: 76px; /* pill 아래 살짝 여백 */
  left: 0;
  padding: 10px 0;
  margin: 0;
  list-style: none;
  min-width: 220px;
  border-radius: 18px;
  background-color: #f5f2f2;
  box-shadow:
    0 12px 30px rgba(0, 0, 0, 0.16),
    0 0 0 1px rgba(255, 255, 255, 0.6) inset;
  z-index: 5;
}

.all-products-dropdown__item {
  padding: 8px 18px;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 300;
  font-size: 16px;
  line-height: 1.4;
  color: #000000;
  white-space: nowrap;
  cursor: pointer;
  transition: background-color 0.18s ease, color 0.18s ease;
}

.all-products-dropdown__item:hover {
  background-color: #e4dfdf;
}

/* 드롭다운 등장 애니메이션 */
.all-products-dropdown-enter-active,
.all-products-dropdown-leave-active {
  transition:
    opacity 0.16s ease,
    transform 0.16s ease;
}

.all-products-dropdown-enter-from,
.all-products-dropdown-leave-to {
  opacity: 0;
  transform: translateY(-4px);
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
  border-radius: 15px;
}

.winter-special-card--second {
  left: 296px;
}

.winter-special-card-text {
  position: absolute;
  left: 40px;
  top: 2620px;
  width: 240px;
  height: 130px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.winter-special-card-text--second {
  left: 298px;
  width: 207px;
  height: 142px;
}

.winter-special-card-text--third {
  left: 552px;
  width: 220px;
  height: 142px;
}

.winter-special-card-text--fourth {
  left: 808px;
  width: 240px;
  height: 142px;
}

.winter-special-card-text--fifth {
  left: 1064px;
  width: 240px;
  height: 142px;
}

.winter-special-card-text--sixth {
  left: 1322px;
  width: 240px;
  height: 132px;
}

.winter-special-card-text--seventh {
  left: 1576px;
  width: 240px;
  height: 132px;
}

.winter-special-circle {
  position: absolute;
  left: 1840px;
  top: 2433px;
  width: 60px;
  height: 60px;
  border-radius: 50px;
  background-color: #ffffff;
  border: 2px solid #000000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.winter-special-circle__icon {
  width: 24px;
  height: 24px;
  color: #000000;
}

.new-products-panel {
  position: absolute;
  left: 38px;
  top: 2842px;
  width: 1206px;
  height: 633px;
  border-radius: 15px;
  background-color: #b8b4ae;
}

.new-products-panel--right {
  left: 1215px;
  width: 601px;
  background-color: #d9d9d9;
}

.membership-heading {
  position: absolute;
  left: 75px;
  top: 2889px;
  width: 774px;
  height: 61px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.membership-heading__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap; /* 한 줄 고정 */
}

@media (max-width: 1200px) {
  .new-products-panel,
  .new-products-panel--right {
    position: static;
    width: 100%;
    max-width: 100%;
    margin-top: 40px;
  }

  .membership-heading,
  .membership-body,
  .membership-logo-text,
  .membership-benefit-heading,
  .membership-benefit-list,
  .membership-benefit-cta {
    position: static;
    width: 100%;
    max-width: 100%;
    left: auto;
    top: auto;
  }

  .membership-heading {
    margin-top: 32px;
  }

  .membership-logo-text {
    margin-top: 32px;
  }

  .membership-benefit-heading {
    margin-top: 40px;
  }

  .membership-benefit-cta {
    margin-top: 24px;
  }

  .membership-heading__text {
    font-size: 32px;
  }

  .membership-body__line {
    font-size: 20px;
  }

  .membership-logo-text__inner {
    font-size: 40px;
  }

  .membership-benefit-heading__text {
    font-size: 28px;
  }

  .membership-benefit-list__item {
    font-size: 22px;
  }

  .membership-benefit-cta__line {
    font-size: 22px;
  }
}

.membership-body {
  position: absolute;
  left: 75px;
  top: 2990px;
  width: 511px;
  max-width: 100%;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.membership-body__line {
  margin: 0;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 30px;
  line-height: 1.4;
  letter-spacing: 0;
  color: #000000;
}

.membership-body__line--spaced {
  margin-top: 16px;
}

.membership-logo-text {
  position: absolute;
  left: 75px;
  top: 3370px;
  width: 276px;
  height: 85px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.membership-logo-text__inner {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 70px;
  line-height: 1;
  letter-spacing: 0;
  color: #000000;
}

.membership-benefit-heading {
  position: absolute;
  left: 1255px;
  top: 2884px;
  width: 218px;
  height: 54px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.membership-benefit-heading__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 45px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap; /* 한 줄 고정 */
}

.membership-benefit-list {
  position: absolute;
  left: 1255px;
  top: 2990px;
  width: 508px;
  max-width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 6px 16px;
}

.membership-benefit-list__item {
  margin: 0;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 30px;
  line-height: 1.4;
  letter-spacing: 0;
  color: #000000;
}

.membership-benefit-cta {
  position: absolute;
  left: 1255px;
  top: 3185px;
  width: 324px;
  max-width: 100%;
  height: 96px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.membership-benefit-cta__text {
  display: flex;
  flex-direction: column;
}

.membership-benefit-cta__line {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 500; /* Medium */
  font-size: 40px;
  line-height: 1.1;
  letter-spacing: 0;
  color: #000000;
}

.membership-benefit-cta__line--highlight {
  margin-top: 4px;
}

.membership-benefit-cta__icon-wrap {
  width: 60px;
  height: 60px;
  border-radius: 50px;
  background-color: #000000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.membership-benefit-cta__icon {
  width: 28px;
  height: 28px;
  color: #ffffff;
}

.new-products-link {
  position: absolute;
  left: 40px;
  top: 3588px;
  width: 202px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.new-products-link__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.bottom-extra-panel {
  position: absolute;
  left: 36.5px;
  top: 3649px;
  width: 661px;
  height: 924px;
  border-radius: 15px;
  background-color: #d9d9d9;
  box-shadow: 0 0 0 1px #ffffff inset;
}

.bottom-extra-panel--right {
  left: 698px;
  top: 3649px;
  width: 671px;
  height: 414px;
}

.bottom-extra-panel--right-lower {
  left: 698px;
  top: 4063px;
  width: 671px;
  height: 510px;
}

.bottom-extra-panel--far-right {
  left: 1369px;
  top: 3649px;
  width: 447px;
  height: 924px;
}

.membership-coupon-text {
  position: absolute;
  left: 725px;
  top: 3973px;
  width: 480px;
  height: 72px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.membership-coupon-text__line {
  margin: 0;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap; /* 자동 줄바꿈 방지, <br> 위치에서만 줄바꿈 */
}

.membership-early-access-text {
  position: absolute;
  left: 725px;
  top: 4484px;
  width: 464px;
  height: 72px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.membership-early-access-text__line {
  margin: 0;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap; /* 자동 줄바꿈 방지, <br> 위치에서만 줄바꿈 */
}

.membership-design-text {
  position: absolute;
  left: 1387px; /* 오른쪽 세로 패널(1369px, 447px) 기준 가로 중앙 정렬 */
  top: 4440px; /* 한 번 더 위로 조정 */
  width: 412px;
  height: 72px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.membership-design-text__line {
  margin: 0;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.3;
  letter-spacing: 0;
  color: #000000;
  white-space: normal; /* 블록 안에서만 자연스럽게 줄바꿈 */
  overflow-wrap: break-word; /* 너무 길면 단어 중간에서라도 줄바꿈 */
}

.bottom-new-products-link {
  position: absolute;
  left: 53px;
  top: 4460px;
  width: auto;
  height: auto;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  gap: 8px;
  cursor: pointer;
  transition: transform 0.15s ease, opacity 0.15s ease;
}

.bottom-new-products-link__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.bottom-new-products-link__icon {
  width: 36px;
  height: 36px;
  color: #000000;
}

.bottom-new-products-link:hover {
  transform: translateX(4px);
  opacity: 0.9;
}

.bottom-footer-bg {
  position: absolute;
  left: 0;
  top: 4709px;
  width: 1929px;
  height: 633px;
  background-color: #f3f3f3;
}

.bottom-footer-scroll {
  position: absolute;
  left: 36px;
  top: 4756px;
  width: calc(100% - 72px); /* 양 옆 36px 여백 유지 */
  height: 513px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  padding-bottom: 16px; /* iOS에서 마지막 카드 잘리지 않도록 여유 */
  -webkit-overflow-scrolling: touch; /* 모바일 부드러운 스크롤 */

  /* 스크롤바 최소화 */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE, Edge */
}

.bottom-footer-scroll::-webkit-scrollbar {
  display: none; /* Chrome, Safari */
}

.bottom-footer-card {
  flex: 0 0 437px; /* 카드 폭 고정 */
  height: 513px;
  border-radius: 15px;
  background-color: #d9d9d9;
  position: relative; /* 내부 고정 요소들의 기준 */
  overflow: hidden;
}

.bottom-footer-card__label {
  position: absolute;
  left: 24px;
  top: 24px;
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.bottom-footer-card__icon {
  position: absolute;
  left: 24px;
  bottom: 24px;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  border-radius: 999px;
  background-color: #000000;
  cursor: pointer;
}

.bottom-footer-card__icon-svg {
  width: 32px;
  height: 32px;
  color: #ffffff;
}

.bottom-large-panel {
  position: absolute;
  left: 41px;
  top: 5463px;
  width: 1845px;
  height: 868px;
  border-radius: 15px;
  background-color: #d9d9d9;
}

.bottom-large-panel__play {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 96px;
  height: 96px;
  border-radius: 999px;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.65);
  cursor: pointer;
}

.bottom-large-panel__play-svg {
  width: 40px;
  height: 40px;
  color: #ffffff;
}

.bottom-final-bg {
  position: absolute;
  left: 0;
  top: 6471px;
  width: 1929px;
  height: 616px;
  background-color: #f3f3f3;
}

.bottom-final-scroll {
  position: absolute;
  left: 41px;
  top: 6522px;
  width: calc(100% - 82px); /* 양 옆 여백 유지 */
  height: 513px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 12px; /* 기존 카드 간 간격 (437폭 + 12px ≒ 449 간격) */
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.bottom-final-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.bottom-final-card {
  flex: 0 0 437px;
  width: 437px;
  height: 513px;
  border-radius: 15px;
  background-color: #d9d9d9;
}

.bottom-final-heading {
  position: absolute;
  left: 41px;
  top: 7221px;
  width: 326px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.bottom-final-heading__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.bottom-final-subheading {
  position: absolute;
  left: 41px;
  top: 7271px;
  width: 256px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.bottom-final-subheading__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 20px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.bottom-interior-scroll {
  position: absolute;
  left: 41px;
  top: 7302px;
  width: calc(100% - 82px);
  height: 907px;
  display: block;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none;
  -ms-overflow-style: none;
}

.bottom-interior-scroll::-webkit-scrollbar {
  display: none;
}

.bottom-interior-track {
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
}

.bottom-final-large-card {
  flex: 0 0 516px;
  width: 516px;
  height: 907px;
  border-radius: 15px;
  background-color: #d9d9d9;
}

.living-room-heading {
  position: absolute;
  left: 41px;
  top: 8355px;
  width: 301px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.living-room-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.kitchen-heading {
  position: absolute;
  left: 41px;
  top: 9168px;
  width: 188px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.kitchen-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.bedroom-heading {
  position: absolute;
  left: 41px;
  top: 9936px;
  width: 237px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.bedroom-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.porch-heading {
  position: absolute;
  left: 41px;
  top: 10721px;
  width: 144px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.porch-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.balcony-heading {
  position: absolute;
  left: 41px;
  top: 11503px;
  width: 194px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.balcony-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.toilet-heading {
  position: absolute;
  left: 41px;
  top: 12287px;
  width: 134px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.toilet-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.dressingroom-heading {
  position: absolute;
  left: 41px;
  top: 13071px;
  width: 355px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.dressingroom-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.kitchen-heading {
  position: absolute;
  left: 41px;
  top: 9168px;
  width: 188px;
  height: 83px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.kitchen-heading__text {
  font-family: 'Gotu', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 50px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.living-room-scroll {
  position: absolute;
  left: 41px;
  top: 8438px;
  width: calc(100% - 82px);
  height: 572px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.living-room-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.living-room-card,
.kitchen-card,
.bedroom-card,
.porch-card,
.balcony-card,
.toilet-card,
.dressingroom-card {
  flex: 0 0 396px;
  width: 396px;
  height: 572px;
  border-radius: 15px;
  background-color: #d9d9d9;
}

.kitchen-scroll {
  position: absolute;
  left: 41px;
  top: 9251px;
  width: calc(100% - 82px);
  height: 572px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.kitchen-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.bedroom-scroll {
  position: absolute;
  left: 41px;
  top: 10019px;
  width: calc(100% - 82px);
  height: 572px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.bedroom-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.porch-scroll {
  position: absolute;
  left: 41px;
  top: 10804px;
  width: calc(100% - 82px);
  height: 572px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.porch-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.balcony-scroll {
  position: absolute;
  left: 41px;
  top: 11586px;
  width: calc(100% - 82px);
  height: 572px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.balcony-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.toilet-scroll {
  position: absolute;
  left: 41px;
  top: 12370px;
  width: calc(100% - 82px);
  height: 572px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.toilet-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.dressingroom-scroll {
  position: absolute;
  left: 41px;
  top: 13154px;
  width: calc(100% - 82px);
  height: 572px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.dressingroom-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.bottom-last-bg {
  position: absolute;
  left: -2px;
  top: 13901px;
  width: 1925px;
  height: 671px;
  background-color: #f3f3f3;
}

.bottom-last-link {
  position: absolute;
  left: 39px;
  top: 13938px;
  width: 436px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.bottom-last-link__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 400; /* Regular */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

.bottom-last-card {
  flex: 0 0 436px;
  width: 436px;
  height: 513px;
  border-radius: 15px;
  background-color: #d9d9d9;
}

.bottom-last-card--cta {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 16px;
  border: none;
  padding: 32px 24px;
  background-color: #d9d9d9;
  cursor: pointer;
}

.bottom-last-cta__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-size: 26px;
  line-height: 1.3;
  font-weight: 500;
  color: #111111;
  text-align: center;
  white-space: normal;
}

.bottom-last-cta__icon-wrap {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.bottom-last-cta__icon {
  width: 28px;
  height: 28px;
  color: #111111;
  transition:
    color 0.2s ease,
    transform 0.2s ease;
}

.bottom-last-card--cta:hover .bottom-last-cta__text,
.bottom-last-card--cta:focus-visible .bottom-last-cta__text {
  color: #000000;
}

.bottom-last-card--cta:hover .bottom-last-cta__icon,
.bottom-last-card--cta:focus-visible .bottom-last-cta__icon {
  color: #000000;
  transform: translateY(-2px);
}

.bottom-last-scroll {
  position: absolute;
  left: 39px;
  top: 13990px;
  width: calc(100% - 78px);
  height: 513px;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 24px;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-overflow-scrolling: touch;

  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE/Edge */
}

.bottom-last-scroll::-webkit-scrollbar {
  display: none; /* Chrome/Safari */
}

.bottom-extra-bg {
  position: absolute;
  left: -2px;
  top: 14669px;
  width: 1927px;
  height: 528px;
  border-radius: 10px;
  background-color: #d9d9d9;
}

/* 카테고리 아이콘 + 텍스트를 하나의 컴포넌트처럼 묶은 wrapper */
.category-item {
  position: absolute;
  top: 189px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  transition:
    transform 0.22s ease-out,
    opacity 0.22s ease-out;
  opacity: 0.9;
}

.category-item--1  { left: 40px; }
.category-item--2  { left: 150px; }
.category-item--3  { left: 313px; }
.category-item--4  { left: 462px; }
.category-item--5  { left: 607px; }
.category-item--6  { left: 816px; }
.category-item--7  { left: 987px; }
.category-item--8  { left: 1215px; }
.category-item--9  { left: 1443px; }
.category-item--10 { left: 1595px; }
.category-item--11 { left: 1755px; }

.category-item__icon {
  width: 69px;
  height: 69px;
  border-radius: 50%;
  background-color: #d9d9d9;
  transition: filter 0.22s ease-out;
}

.category-item__label {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100;
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #111111;
  white-space: nowrap;
  transition: color 0.22s ease-out;
}

.category-item:hover,
.category-item:focus-visible {
  transform: translateY(-2px) scale(1.04);
  opacity: 1;
}

.category-item:hover .category-item__icon,
.category-item:focus-visible .category-item__icon {
  filter: brightness(0.94);
}

.category-item:hover .category-item__label,
.category-item:focus-visible .category-item__label {
  color: #000000;
}

.category-item:active {
  transform: translateY(-1px) scale(1.01);
}

/* ---------------------------------------------
 * Global interactive card / surface animations
 * ------------------------------------------ */

/* 카드/패널 공통: 부드러운 호버 애니메이션 (하단 혜택 카드는 제외) */
.bottom-final-card,
.bottom-final-large-card,
.winter-special-card,
.color-card-red,
.color-card-orange,
.color-card-yellow,
.color-card-green,
.color-card-blue,
.color-card-purple,
.color-card-black,
.living-room-card,
.kitchen-card,
.bedroom-card,
.porch-card,
.balcony-card,
.toilet-card,
.dressingroom-card,
.bottom-large-panel,
.bottom-last-card,
.bottom-extra-bg {
  transition:
    transform 0.22s ease,
    box-shadow 0.22s ease,
    background-color 0.22s ease,
    filter 0.22s ease;
  will-change: transform, box-shadow, filter;
}

.bottom-final-card:hover,
.bottom-final-large-card:hover,
.winter-special-card:hover,
.color-card-red:hover,
.color-card-orange:hover,
.color-card-yellow:hover,
.color-card-green:hover,
.color-card-blue:hover,
.color-card-purple:hover,
.color-card-black:hover,
.living-room-card:hover,
.kitchen-card:hover,
.bedroom-card:hover,
.porch-card:hover,
.balcony-card:hover,
.toilet-card:hover,
.dressingroom-card:hover,
.bottom-large-panel:hover,
.bottom-last-card:hover,
.bottom-extra-bg:hover {
  transform: translateY(-6px);
  box-shadow:
    0 18px 40px rgba(0, 0, 0, 0.16),
    0 0 0 1px rgba(255, 255, 255, 0.2) inset;
  filter: brightness(0.97);
}

/* 텍스트 링크: 살짝 어두워지고 오른쪽으로 미묘하게 이동 */
.bottom-new-products-link,
.bottom-last-link {
  transition: transform 0.18s ease, opacity 0.18s ease, color 0.18s ease;
}

.bottom-new-products-link:hover,
.bottom-last-link:hover {
  transform: translateX(4px);
  opacity: 0.9;
}

.bottom-last-link__text {
  position: relative;
}

.bottom-last-link__text::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -4px;
  width: 0;
  height: 1px;
  background-color: #000000;
  transition: width 0.2s ease;
}

.bottom-last-link:hover .bottom-last-link__text::after {
  width: 100%;
}

.winter-special-card-text__brand {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 700; /* Bold */
  font-size: 30px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
}

.winter-special-card-text__name {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 20px;
  line-height: 1.4;
  letter-spacing: 0;
  color: #000000;
}

.winter-special-card-text__price {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 30px;
  line-height: 1.3;
  letter-spacing: 0;
  color: #000000;
}

.winter-special-card-text__original {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 20px;
  line-height: 1.4;
  letter-spacing: 0;
  color: #000000;
}

.winter-special-card {
  position: absolute;
  left: 40px;
  top: 2305px;
  width: 240px;
  height: 305px;
  background-color: #d9d9d9;
  border-radius: 15px;
}

.winter-special-card--second {
  left: 296px;
}

.winter-special-card--third {
  left: 552px;
}

.winter-special-card--fourth {
  left: 808px;
}

.winter-special-card--fifth {
  left: 1064px;
}

.winter-special-card--sixth {
  left: 1320px;
}

.winter-special-card--seventh {
  left: 1576px;
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

.tenth-extra-icon {
  position: absolute;
  left: 1770px;
  top: 189px;
  width: 69px;
  height: 69px;
  border-radius: 50px;
  background-color: #ede9e9;
}

/* ---------------------------------------------
 * Category icon + label minimal interactions
 * ------------------------------------------ */

/* 아이콘 원: hover 시 살짝 위로 + scale, 톤만 조금 진하게 */
.single-category-icon,
.second-category-icon,
.third-category-icon,
.fourth-category-icon,
.fifth-category-icon,
.sixth-category-icon,
.seventh-category-icon,
.eighth-category-icon,
.ninth-category-icon,
.tenth-category-icon,
.tenth-extra-icon {
  transition:
    transform 0.22s ease-out,
    filter 0.22s ease-out;
  cursor: pointer;
}

.single-category-icon:hover,
.second-category-icon:hover,
.third-category-icon:hover,
.fourth-category-icon:hover,
.fifth-category-icon:hover,
.sixth-category-icon:hover,
.seventh-category-icon:hover,
.eighth-category-icon:hover,
.ninth-category-icon:hover,
.tenth-category-icon:hover,
.tenth-extra-icon:hover {
  transform: translateY(-2px) scale(1.04);
  filter: brightness(0.94);
}

.single-category-icon:active,
.second-category-icon:active,
.third-category-icon:active,
.fourth-category-icon:active,
.fifth-category-icon:active,
.sixth-category-icon:active,
.seventh-category-icon:active,
.eighth-category-icon:active,
.ninth-category-icon:active,
.tenth-category-icon:active,
.tenth-extra-icon:active {
  transform: translateY(-1px) scale(1.01);
  filter: brightness(0.9);
}

.tenth-extra-label {
  position: absolute;
  left: 1782px;
  top: 266px;
  width: 46px;
  height: 30px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
}

.tenth-extra-label__text {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
    sans-serif;
  font-weight: 100; /* Thin */
  font-size: 25px;
  line-height: 1.2;
  letter-spacing: 0;
  color: #000000;
  white-space: nowrap;
}

/* 카테고리 텍스트: 기본은 약간 연한 느낌, hover 시 진해지고 살짝 위로 */
.single-category-label,
.second-category-label,
.third-category-label,
.fourth-category-label,
.fifth-category-label,
.sixth-category-label,
.seventh-category-label,
.eighth-category-label,
.ninth-category-label,
.tenth-category-label,
.tenth-extra-label {
  cursor: pointer;
  transition: transform 0.22s ease-out, opacity 0.22s ease-out;
  opacity: 0.9;
}

.single-category-label:hover,
.second-category-label:hover,
.third-category-label:hover,
.fourth-category-label:hover,
.fifth-category-label:hover,
.sixth-category-label:hover,
.seventh-category-label:hover,
.eighth-category-label:hover,
.ninth-category-label:hover,
.tenth-category-label:hover,
.tenth-extra-label:hover {
  transform: translateY(-1px);
  opacity: 1;
}

.single-category-label__text,
.second-category-label__text,
.third-category-label__text,
.fourth-category-label__text,
.fifth-category-label__text,
.sixth-category-label__text,
.seventh-category-label__text,
.eighth-category-label__text,
.ninth-category-label__text,
.tenth-category-label__text,
.tenth-extra-label__text {
  transition: color 0.22s ease-out;
}

.single-category-label:hover .single-category-label__text,
.second-category-label:hover .second-category-label__text,
.third-category-label:hover .third-category-label__text,
.fourth-category-label:hover .fourth-category-label__text,
.fifth-category-label:hover .fifth-category-label__text,
.sixth-category-label:hover .sixth-category-label__text,
.seventh-category-label:hover .seventh-category-label__text,
.eighth-category-label:hover .eighth-category-label__text,
.ninth-category-label:hover .ninth-category-label__text,
.tenth-category-label:hover .tenth-category-label__text,
.tenth-extra-label:hover .tenth-extra-label__text {
  color: #111111;
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
  position: relative;
  cursor: pointer;
  transition: color 0.18s ease, transform 0.18s ease, opacity 0.18s ease;
}

.nav-item--label {
  /* 필요하면 나중에 굵기/색상 차이 줄 수 있음 */
}

/* 상단 내비게이션 항목: 호버 시 밑줄 + 살짝 움직이는 애니메이션 */
.nav-item::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -4px;
  width: 0;
  height: 2px;
  background-color: #000000;
  opacity: 0;
  transform-origin: left;
  transition:
    width 0.2s ease,
    opacity 0.2s ease,
    transform 0.2s ease;
}

.nav-item:hover {
  transform: translateY(-1px);
  opacity: 0.9;
}

.nav-item:hover::after {
  width: 100%;
  opacity: 1;
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