<script setup lang="ts">
import { MapPin, Star } from 'lucide-vue-next'
import { ref } from 'vue'
import ScenicRouteCard from './ScenicRouteCard.vue'
import CyclingRouteCard from './CyclingRouteCard.vue'
import FoodRouteCard from './FoodRouteCard.vue'

const scenic = {
  index: 2,
  title: '景点环线',
  subtitle: 'Scenic Loop',
  description: '串联社村核心景点，漫步古村落，感受千年文化底蕴，适合全家出游。',
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
}

const food = {
  index: 4,
  title: '美食线',
  subtitle: 'Food Trail',
  description: '品味地道农家菜，探访特色小吃摊，寻找舌尖上的社村，味蕾的极致享受。',
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
}

const activeRoute = ref<number>(2)
const favorites = ref<Set<number>>(new Set())
const selectedRoute = ref<typeof scenic | typeof cycling | typeof food | null>(null)

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
      class="fixed inset-0 bg-foreground/50 backdrop-blur-sm flex items-center justify-center z-50 p-4 animate-fade-in"
      @click="closeDetail"
    >
      <div
        class="bg-card rounded-2xl max-w-lg w-full overflow-hidden shadow-2xl animate-slide-up"
        @click.stop
      >
        <!-- Modal header -->
        <div class="relative h-48 overflow-hidden">
          <img
            :src="selectedRoute.images[0]"
            :alt="selectedRoute.title"
            class="absolute inset-0 w-full h-full object-cover"
          />
          <div class="absolute inset-0 bg-gradient-to-t from-card via-card/40 to-transparent" />
          <div class="absolute top-4 left-4 w-10 h-10 rounded-xl bg-card/60 backdrop-blur-sm flex items-center justify-center">
            <span class="text-xl font-bold text-foreground">{{ selectedRoute.index }}</span>
          </div>
          <button
            class="absolute top-4 right-4 w-8 h-8 rounded-full bg-card/60 backdrop-blur-sm flex items-center justify-center hover:bg-card/80 transition-colors"
            @click="closeDetail"
            aria-label="关闭"
          >
            <svg class="w-4 h-4 text-foreground" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
              <path d="M18 6L6 18M6 6l12 12" />
            </svg>
          </button>
          <div class="absolute bottom-4 left-4">
            <h3 class="text-2xl font-bold text-foreground">{{ selectedRoute.title }}</h3>
            <p class="text-sm text-muted-foreground">{{ selectedRoute.subtitle }}</p>
          </div>
        </div>

        <!-- Modal body -->
        <div class="p-6">
          <p class="text-muted-foreground leading-relaxed mb-6">{{ selectedRoute.description }}</p>

          <div class="grid grid-cols-3 gap-4 mb-6">
            <div class="text-center p-3 rounded-lg bg-accent">
              <div class="text-lg font-bold">{{ selectedRoute.distance }}</div>
              <div class="text-xs text-muted-foreground">路线长度</div>
            </div>
            <div class="text-center p-3 rounded-lg bg-accent">
              <div class="text-lg font-bold">{{ selectedRoute.duration }}</div>
              <div class="text-xs text-muted-foreground">预计用时</div>
            </div>
            <div class="text-center p-3 rounded-lg bg-accent">
              <div class="text-lg font-bold">{{ selectedRoute.difficulty }}</div>
              <div class="text-xs text-muted-foreground">难度等级</div>
            </div>
          </div>

          <div class="mb-6">
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

          <div class="flex flex-wrap gap-2 mb-6">
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
              @click="closeDetail"
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
