<script setup lang="ts">
import { MapPin, Star, ChevronLeft, ChevronRight } from 'lucide-vue-next'
import { ref, watch } from 'vue'
import ScenicRouteCard from './ScenicRouteCard.vue'
import CyclingRouteCard from './CyclingRouteCard.vue'
import FoodRouteCard from './FoodRouteCard.vue'

const scenic = {
  index: 2,
  title: '景点环线',
  subtitle: 'Scenic Loop',
  description: '串联佘村核心景点，漫步古村落，感受千年文化底蕴，适合全家出游。',
  images: ['/images/图1.png', '/images/图2.png', '/images/图3.png'],
  color: 'bg-scenic/10',
  bgColor: 'bg-scenic',
  borderColor: 'border-scenic/30',
  textColor: 'text-scenic',
  tags: ['古村落', '文化遗迹', '摄影打卡'],
  distance: '8.5km',
  duration: '半天',
  difficulty: '简单',
  spots: ['古祠堂', '千年古树', '观景台', '民俗博物馆'],
  navigationUrl: 'https://m.amap.com/navigation/carmap/__r=31.981616971469396,118.93030911684038,%E6%B1%9F%E5%AE%81%E4%BD%98%E6%9D%91,31.982305,118.929565,%E4%BD%98%E9%A6%99%E5%92%96%E5%95%A1,,0,0,,,,%2Bf80basxS37a%2BgBbMZ4K%2FPwLcOLl5gpFT%2Bh6uDD0CPi9vmJnZN0Bfjf8lA4DNlTkG7ZNPc6aOjZlyYto3NGevyLpgk3aOBbvg5%2Fbd%2F8rweEK9Es8HrdpK7nUNuJAgx4X,31.978855%7C31.983233,118.930607%7C118.930782,%E7%88%AA%E7%88%AA%E7%89%A7%E5%9C%BA%7C%E5%B0%8F%E6%BB%A1%E7%B3%96%E6%B0%B4%E9%93%BA&src=app_share&callnative=1&callapp=0&userRelationToken=a4aa80fe3da511f19ca600163e081a5a1&share_type=url&share_from=drive_CarResultPage&share_from_type=AJX&share_bizParams=%7B%22naviDistance%22%3A4%2C%22share_content%22%3A%22route%22%2C%22trigger%22%3A%22click%22%2C%22end_poiid%22%3A%22B0KDLS8BBZ%22%7D&share_lastClickSpm=amap.P00016.0.D092&share_bid=t9bg16n9c58jfs3m3fcmhgonaiagjmg0907147a&saddr=118.93030911684038,31.981616971469396,%E6%B1%9F%E5%AE%81%E4%BD%98%E6%9D%91&daddr=118.929565,31.982305,%E4%BD%98%E9%A6%99%E5%92%96%E5%95%A1&sort=dist&shareParam=%2Bf80basxS37a%2BgBbMZ4K%2FPwLcOLl5gpFT%2Bh6uDD0CPi9vmJnZN0Bfjf8lA4DNlTkG7ZNPc6aOjZlyYto3NGevyLpgk3aOBbvg5%2Fbd%2F8rweEK9Es8HrdpK7nUNuJAgx4X&viaaddr=118.930607%7C118.930782,31.978855%7C31.983233,%E7%88%AA%E7%88%AA%E7%89%A7%E5%9C%BA%7C%E5%B0%8F%E6%BB%A1%E7%B3%96%E6%B0%B4%E9%93%BA',
}

const cycling = {
  index: 3,
  title: '骑行线',
  subtitle: 'Cycling Trail',
  description: '穿越山水田园的骑行路线，沿途风景如画，呼吸自然清新空气，活力满分。',
  images: ['/images/图3.png'],
  color: 'bg-cycling/10',
  bgColor: 'bg-cycling',
  borderColor: 'border-cycling/30',
  textColor: 'text-cycling',
  tags: ['户外运动', '田园风光', '健康养生'],
  distance: '15km',
  duration: '一天',
  difficulty: '中等',
  spots: ['竹林步道', '溪谷桥', '观景亭', '茶园'],
  navigationUrl: '',
}

const food = {
  index: 4,
  title: '美食线',
  subtitle: 'Food Trail',
  description: '品味地道农家菜，探访特色小吃摊，寻找舌尖上的佘村，味蕾的极致享受。',
  images: ['/images/图4.png'],
  color: 'bg-food/10',
  bgColor: 'bg-food',
  borderColor: 'border-food/30',
  textColor: 'text-food',
  tags: ['农家菜', '特色小吃', '美食地图'],
  distance: '5km',
  duration: '半天',
  difficulty: '简单',
  spots: ['老街小吃', '农家宴', '手工豆腐坊', '茶庄'],
  navigationUrl: '',
}

const activeRoute = ref<number>(2)
const favorites = ref<Set<number>>(new Set())
const selectedRoute = ref<typeof scenic | typeof cycling | typeof food | null>(null)
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

function openRoute(route: typeof scenic) {
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
        <p class="text-sm text-muted-foreground mt-1">三条特色路线，总有一条适合你</p>
      </div>
    </div>

    <!-- Cards -->
    <div class="flex flex-col md:flex-row gap-5 mb-6">
      <ScenicRouteCard
        v-bind="scenic"
        :is-active="activeRoute === scenic.index"
        :is-favorite="favorites.has(scenic.index)"
        @click="openRoute(scenic)"
        @favorite="toggleFavorite(scenic.index)"
      />
      <CyclingRouteCard
        v-bind="cycling"
        :is-active="activeRoute === cycling.index"
        :is-favorite="favorites.has(cycling.index)"
        @click="openRoute(cycling)"
        @favorite="toggleFavorite(cycling.index)"
      />
      <FoodRouteCard
        v-bind="food"
        :is-active="activeRoute === food.index"
        :is-favorite="favorites.has(food.index)"
        @click="openRoute(food)"
        @favorite="toggleFavorite(food.index)"
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
