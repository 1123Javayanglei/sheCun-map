<script setup lang="ts">
import { MapPin, Star, ChevronLeft, ChevronRight } from 'lucide-vue-next'
import { ref, watch } from 'vue'
import RouteCard from './RouteCard.vue'

// 路线①：亲子线
const family = {
  index: 1,
  title: '亲子线',
  subtitle: 'Family Fun Trail',
  description: '和孩子一起，感受金陵古村的童趣与温情。从古银杏到爪爪牧场，从共享菜园到泥房子手作，全程充满亲子乐趣。',
  images: [
    '/images/poi/村口古银杏树/1.jpg',
    '/images/poi/村口古银杏树/2.jpg',
    '/images/poi/村口古银杏树/3.jpg',
    '/images/poi/爪爪牧场/1.jpg',
    '/images/poi/泥房子ni house/1.jpg',
    '/images/poi/泥房子ni house/2.jpg',
    '/images/poi/泥房子ni house/3.jpg',
    '/images/poi/共享菜园/1.jpg',
    '/images/poi/露天野营基地/1.jpg',
    '/images/poi/露天野营基地/2.jpg',
    '/images/poi/露天野营基地/3.jpg',
    '/images/poi/儿童游乐设施/1.jpg',
    '/images/poi/儿童游乐设施/2.jpg',
    '/images/poi/儿童游乐设施/3.jpg',
  ],
  color: 'bg-pink-500/10',
  bgColor: 'bg-pink-500',
  borderColor: 'border-pink-500/30',
  textColor: 'text-pink-600',
  tags: ['亲子互动', '萌宠体验', '自然教育'],
  distance: '5km',
  duration: '半天至一天',
  difficulty: '简单',
  spots: ['村口古银杏', '双龙湖栈道', '爪爪牧场', '共享菜园', '泥房子手作', '露营基地'],
  navigationUrl: '',
}

// 路线②：骑行线
const cycling = {
  index: 2,
  title: '骑行线',
  subtitle: 'Cycling Trail',
  description: '彩虹公路+环湖骑行道+山野穿越，一路风景一路自由。春秋两季最佳，春季有油菜花，秋季秋色怡人。',
  images: [
    '/images/poi/油菜花海/1.jpg',
    '/images/poi/油菜花海/2.jpg',
    '/images/poi/油菜花海/3.jpg',
    '/images/poi/佘村土菜馆/1.jpg',
    '/images/poi/佘村土菜馆/2.jpg',
    '/images/poi/佘村土菜馆/3.jpg',
    '/images/poi/野有also coffee/1.jpg',
    '/images/poi/野有also coffee/2.jpg',
    '/images/poi/野有also coffee/3.jpg',
    '/images/poi/龙池/1.jpg',
    '/images/poi/龙池/2.jpg',
    '/images/poi/龙池/3.jpg',
  ],
  color: 'bg-cycling/10',
  bgColor: 'bg-cycling',
  borderColor: 'border-cycling/30',
  textColor: 'text-cycling',
  tags: ['户外运动', '田园风光', '健康养生'],
  distance: '8-10km',
  duration: '2-3小时',
  difficulty: '中等',
  spots: ['彩虹公路', '双龙湖环湖道', '油菜花海', '山野路段', '野有also咖啡'],
  navigationUrl: '',
}

// 路线③：美食线
const food = {
  index: 3,
  title: '美食线',
  subtitle: 'Food Trail',
  description: '从烟火集市到私房土菜，从咖啡甜点到特色火锅，吃遍佘村。库谷、懒屋、赤茧三家咖啡馆必打卡。',
  images: [
    '/images/poi/九龙广场/1.jpg',
    '/images/poi/九龙广场/2.jpg',
    '/images/poi/九龙广场/3.jpg',
    '/images/poi/梨园春晓土菜馆/1.jpg',
    '/images/poi/梨园春晓土菜馆/2.jpg',
    '/images/poi/梨园春晓土菜馆/3.jpg',
    '/images/poi/库谷咖啡/1.jpg',
    '/images/poi/库谷咖啡/2.jpg',
    '/images/poi/库谷咖啡/3.jpg',
    '/images/poi/懒屋咖啡/1.jpg',
    '/images/poi/懒屋咖啡/2.jpg',
    '/images/poi/懒屋咖啡/3.jpg',
    '/images/poi/赤茧咖啡/1.jpg',
    '/images/poi/赤茧咖啡/2.jpg',
    '/images/poi/赤茧咖啡/3.jpg',
    '/images/poi/迷你兔蛋糕/1.jpg',
    '/images/poi/迷你兔蛋糕/2.jpg',
    '/images/poi/迷你兔蛋糕/3.jpg',
    '/images/poi/岁月静好土菜馆/1.jpg',
    '/images/poi/岁月静好土菜馆/2.jpg',
    '/images/poi/岁月静好土菜馆/3.jpg',
    '/images/poi/佘村火锅/1.jpg',
    '/images/poi/佘村火锅/2.jpg',
    '/images/poi/佘村火锅/3.jpg',
    '/images/poi/明室咖啡/1.jpg',
    '/images/poi/明室咖啡/2.jpg',
    '/images/poi/明室咖啡/3.jpg',
  ],
  color: 'bg-food/10',
  bgColor: 'bg-food',
  borderColor: 'border-food/30',
  textColor: 'text-food',
  tags: ['农家菜', '特色小吃', '咖啡打卡'],
  distance: '3km',
  duration: '一天',
  difficulty: '简单',
  spots: ['艺术菜场', '梨园春晓', '库谷咖啡', '赤茧咖啡', '迷你兔甜品', '佘村火锅'],
  navigationUrl: '',
}

// 路线④：文化线
const culture = {
  index: 4,
  title: '文化线',
  subtitle: 'Cultural Heritage Trail',
  description: '穿越明清，品读潘氏家族600年的辉煌与传承。参观明代古建筑群，体验汉服换装，感受金陵古风第一村的魅力。',
  images: [
    '/images/poi/佘村明清代古建筑群/1.jpg',
    '/images/poi/佘村明清代古建筑群/2.jpg',
    '/images/poi/佘村明清代古建筑群/3.jpg',
    '/images/poi/潘氏住宅/1.jpg',
    '/images/poi/潘氏住宅/2.jpg',
    '/images/poi/潘氏住宅/3.jpg',
    '/images/poi/潘氏宗祠/1.jpg',
    '/images/poi/潘氏宗祠/2.jpg',
    '/images/poi/潘氏宗祠/3.jpg',
    '/images/poi/李家大院/1.jpg',
    '/images/poi/李家大院/2.jpg',
    '/images/poi/李家大院/3.jpg',
    '/images/poi/熹·游记汉服体验馆/1.jpg',
    '/images/poi/熹·游记汉服体验馆/2.jpg',
    '/images/poi/村口古银杏树/1.jpg',
    '/images/poi/村口古银杏树/2.jpg',
    '/images/poi/村口古银杏树/3.jpg',
  ],
  color: 'bg-amber-600/10',
  bgColor: 'bg-amber-600',
  borderColor: 'border-amber-600/30',
  textColor: 'text-amber-700',
  tags: ['古建筑', '非遗文化', '汉服体验'],
  distance: '2km',
  duration: '3-4小时',
  difficulty: '简单',
  spots: ['古银杏树', '佘村古井', '潘氏住宅', '潘氏宗祠', '李家大院', '熹·游记汉服馆'],
  navigationUrl: '',
}

// 路线⑤：生态线
const ecology = {
  index: 5,
  title: '生态线',
  subtitle: 'Eco Nature Trail',
  description: '山水之间，做一日陶渊明式的归园田居人。晨雾双龙湖、芦苇荡观鸟、龙池徒步、露营发呆，享受山野慢生活。',
  images: [
    '/images/poi/龙池/1.jpg',
    '/images/poi/龙池/2.jpg',
    '/images/poi/龙池/3.jpg',
    '/images/poi/露营基地/1.jpg',
    '/images/poi/露营基地/2.jpg',
    '/images/poi/露营基地/3.jpg',
    '/images/poi/泥房子ni house/1.jpg',
    '/images/poi/泥房子ni house/2.jpg',
    '/images/poi/泥房子ni house/3.jpg',
    '/images/poi/山间古宅咖啡馆/1.jpg',
    '/images/poi/山间古宅咖啡馆/2.jpg',
    '/images/poi/山间古宅咖啡馆/3.jpg',
    '/images/poi/油菜花海/1.jpg',
    '/images/poi/油菜花海/2.jpg',
    '/images/poi/油菜花海/3.jpg',
  ],
  color: 'bg-emerald-600/10',
  bgColor: 'bg-emerald-600',
  borderColor: 'border-emerald-600/30',
  textColor: 'text-emerald-700',
  tags: ['自然风光', '徒步观鸟', '露营体验'],
  distance: '6km',
  duration: '一天',
  difficulty: '简单',
  spots: ['双龙湖晨雾', '芦苇荡观鸟', '龙池徒步', '露营基地', '泥房子手作', '山间古宅咖啡'],
  navigationUrl: '',
}

const activeRoute = ref<number>(1)
const favorites = ref<Set<number>>(new Set())
type RouteType = typeof family | typeof cycling | typeof food | typeof culture | typeof ecology
const selectedRoute = ref<RouteType | null>(null)
const modalImageIndex = ref(0)

watch(selectedRoute, () => {
  modalImageIndex.value = 0
})

function toggleFavorite(index: number) {
  const next = new Set(favorites.value)
  if (next.has(index)) next.delete(index)
  else next.add(index)
  favorites.value = next
}

function openRoute(route: RouteType) {
  activeRoute.value = route.index
  selectedRoute.value = route
}

function closeDetail() {
  selectedRoute.value = null
}

function startNavigation() {
  if (selectedRoute.value && 'navigationUrl' in selectedRoute.value && selectedRoute.value.navigationUrl) {
    window.open(selectedRoute.value.navigationUrl, '_blank')
  }
}

function prevModalImage() {
  if (!selectedRoute.value || selectedRoute.value.images.length <= 1) return
  modalImageIndex.value--
  if (modalImageIndex.value < 0) modalImageIndex.value = selectedRoute.value.images.length - 1
}

function nextModalImage() {
  if (!selectedRoute.value || selectedRoute.value.images.length <= 1) return
  modalImageIndex.value++
  if (modalImageIndex.value >= selectedRoute.value.images.length) modalImageIndex.value = 0
}

function onModalKeyDown(e: KeyboardEvent) {
  if (e.key === 'ArrowLeft') prevModalImage()
  if (e.key === 'ArrowRight') nextModalImage()
  if (e.key === 'Escape') closeDetail()
}

// Touch swipe for image carousel
const containerRef = ref<HTMLElement | null>(null)
let touchStartX = 0
let touchStartY = 0

function onTouchStart(e: TouchEvent) {
  touchStartX = e.touches[0].clientX
  touchStartY = e.touches[0].clientY
}

function onTouchEnd(e: TouchEvent) {
  const diffX = e.changedTouches[0].clientX - touchStartX
  const diffY = e.changedTouches[0].clientY - touchStartY
  // Only trigger swipe if horizontal movement dominates
  if (Math.abs(diffX) > Math.abs(diffY) && Math.abs(diffX) > 50) {
    if (diffX > 0) prevModalImage()
    else nextModalImage()
  }
}
</script>

<template>
  <section>
    <div class="flex items-center justify-between mb-6">
      <div>
        <h2 class="text-xl font-bold">选择你的路线</h2>
        <p class="text-sm text-muted-foreground mt-1">五条特色路线，总有一条适合你</p>
      </div>
    </div>

    <!-- Cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5 mb-6">
      <RouteCard
        v-bind="family"
        :is-active="activeRoute === family.index"
        :is-favorite="favorites.has(family.index)"
        @click="openRoute(family)"
        @favorite="toggleFavorite(family.index)"
      />
      <RouteCard
        v-bind="cycling"
        :is-active="activeRoute === cycling.index"
        :is-favorite="favorites.has(cycling.index)"
        @click="openRoute(cycling)"
        @favorite="toggleFavorite(cycling.index)"
      />
      <RouteCard
        v-bind="food"
        :is-active="activeRoute === food.index"
        :is-favorite="favorites.has(food.index)"
        @click="openRoute(food)"
        @favorite="toggleFavorite(food.index)"
      />
      <RouteCard
        v-bind="culture"
        :is-active="activeRoute === culture.index"
        :is-favorite="favorites.has(culture.index)"
        @click="openRoute(culture)"
        @favorite="toggleFavorite(culture.index)"
      />
      <RouteCard
        v-bind="ecology"
        :is-active="activeRoute === ecology.index"
        :is-favorite="favorites.has(ecology.index)"
        @click="openRoute(ecology)"
        @favorite="toggleFavorite(ecology.index)"
      />
    </div>

    <!-- Route Detail Modal -->
    <div
      v-if="selectedRoute"
      class="fixed inset-0 bg-foreground/50 backdrop-blur-sm flex items-start justify-center z-50 p-0 md:p-4 overflow-y-auto animate-fade-in"
      @click="closeDetail"
      @keydown="onModalKeyDown"
    >
      <div
        class="bg-card w-full md:max-w-3xl md:rounded-2xl overflow-hidden shadow-2xl animate-slide-up flex flex-col my-8"
        @click.stop
      >
        <!-- Image area - scrollable and swipeable -->
        <div
          ref="containerRef"
          class="relative bg-accent"
          @touchstart="onTouchStart"
          @touchend="onTouchEnd"
        >
          <img
            :src="selectedRoute.images[modalImageIndex]"
            :alt="selectedRoute.title"
            class="w-full h-auto block"
          />

          <!-- Carousel controls -->
          <template v-if="selectedRoute.images.length > 1">
            <button
              class="absolute left-3 top-1/2 -translate-y-1/2 w-10 h-10 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
              @click.stop="prevModalImage"
              aria-label="上一张"
            >
              <ChevronLeft class="w-6 h-6" />
            </button>
            <button
              class="absolute right-3 top-1/2 -translate-y-1/2 w-10 h-10 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
              @click.stop="nextModalImage"
              aria-label="下一张"
            >
              <ChevronRight class="w-6 h-6" />
            </button>
            <!-- Dots -->
            <div class="absolute bottom-4 left-1/2 -translate-x-1/2 flex gap-2 z-10">
              <span
                v-for="(_, i) in selectedRoute.images"
                :key="i"
                :class="[
                  'block rounded-full transition-all shadow-sm',
                  i === modalImageIndex ? 'w-2.5 h-2.5 bg-white' : 'w-2 h-2 bg-white/60'
                ]"
              />
            </div>
          </template>
        </div>

        <!-- Info bar overlay -->
        <div class="relative px-4 py-3 bg-card border-b border-border flex-shrink-0">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 rounded-lg bg-primary/20 flex items-center justify-center flex-shrink-0">
              <span class="text-sm font-bold text-primary">{{ selectedRoute.index }}</span>
            </div>
            <div class="flex-1 min-w-0">
              <h3 class="text-base font-bold text-foreground truncate">{{ selectedRoute.title }}</h3>
              <p class="text-xs text-muted-foreground">{{ selectedRoute.subtitle }}</p>
            </div>
            <button
              class="w-8 h-8 rounded-full hover:bg-accent flex items-center justify-center transition-colors flex-shrink-0"
              @click="closeDetail"
              aria-label="关闭"
            >
              <svg class="w-5 h-5 text-foreground" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                <path d="M18 6L6 18M6 6l12 12" />
              </svg>
            </button>
          </div>
        </div>

        <!-- Modal body - scrollable -->
        <div class="p-5 overflow-auto flex-1 min-h-0">
          <p class="text-sm text-muted-foreground leading-relaxed mb-5">{{ selectedRoute.description }}</p>

          <div class="grid grid-cols-3 gap-3 mb-5">
            <div class="text-center p-3 rounded-lg bg-accent">
              <div class="text-base font-bold">{{ selectedRoute.distance }}</div>
              <div class="text-xs text-muted-foreground mt-0.5">路线长度</div>
            </div>
            <div class="text-center p-3 rounded-lg bg-accent">
              <div class="text-base font-bold">{{ selectedRoute.duration }}</div>
              <div class="text-xs text-muted-foreground mt-0.5">预计用时</div>
            </div>
            <div class="text-center p-3 rounded-lg bg-accent">
              <div class="text-base font-bold">{{ selectedRoute.difficulty }}</div>
              <div class="text-xs text-muted-foreground mt-0.5">难度等级</div>
            </div>
          </div>

          <div class="mb-5">
            <h4 class="text-sm font-semibold mb-3 flex items-center gap-2">
              <Star class="w-4 h-4 text-food" />
              途经景点
            </h4>
            <div class="flex flex-wrap gap-2">
              <span
                v-for="(spot, i) in selectedRoute.spots"
                :key="i"
                class="inline-flex items-center gap-1 px-3 py-1.5 rounded-lg bg-accent text-sm"
              >
                <MapPin class="w-3.5 h-3.5 text-primary" />
                {{ spot }}
              </span>
            </div>
          </div>

          <div class="flex flex-wrap gap-2 mb-5">
            <span
              v-for="(tag, i) in selectedRoute.tags"
              :key="i"
              :class="`inline-flex items-center px-3 py-1 rounded-md text-xs font-medium ${selectedRoute.color} ${selectedRoute.textColor}`"
            >
              {{ tag }}
            </span>
          </div>

          <div class="flex gap-3">
            <button
              :class="`flex-1 py-2.5 rounded-lg ${selectedRoute.bgColor} text-white font-medium text-sm hover:opacity-90 transition-opacity`"
              @click="startNavigation"
            >
              开始导航
            </button>
            <button
              class="flex-1 py-2.5 rounded-lg border border-border text-sm font-medium hover:bg-accent transition-colors"
              @click="toggleFavorite(selectedRoute.index)"
            >
              {{ favorites.has(selectedRoute.index) ? '已收藏' : '收藏路线' }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
