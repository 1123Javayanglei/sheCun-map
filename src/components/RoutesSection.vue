<script setup lang="ts">
import { Mountain, Bike, UtensilsCrossed, MapPin, Heart, Star, ChevronLeft, ChevronRight } from 'lucide-vue-next'
import { ref } from 'vue'

interface RouteItem {
  index: number
  title: string
  subtitle: string
  description: string
  images: string[]
  color: string
  bgColor: string
  borderColor: string
  textColor: string
  icon: 'mountain' | 'bike' | 'utensils'
  tags: string[]
  distance: string
  duration: string
  difficulty: string
  spots: string[]
}

const routes: RouteItem[] = [
  {
    index: 2,
    title: '景点环线',
    subtitle: 'Scenic Loop',
    description: '串联社村核心景点，漫步古村落，感受千年文化底蕴，适合全家出游。',
    images: ['/images/图1.png', '/images/图2.png', '/images/图3.png'],
    color: 'bg-scenic/10',
    bgColor: 'bg-scenic',
    borderColor: 'border-scenic/30',
    textColor: 'text-scenic',
    icon: 'mountain',
    tags: ['古村落', '文化遗迹', '摄影打卡'],
    distance: '8.5km',
    duration: '半天',
    difficulty: '简单',
    spots: ['古祠堂', '千年古树', '观景台', '民俗博物馆'],
  },
  {
    index: 3,
    title: '骑行线',
    subtitle: 'Cycling Trail',
    description: '穿越山水田园的骑行路线，沿途风景如画，呼吸自然清新空气，活力满分。',
    images: ['/images/图3.png'],
    color: 'bg-cycling/10',
    bgColor: 'bg-cycling',
    borderColor: 'border-cycling/30',
    textColor: 'text-cycling',
    icon: 'bike',
    tags: ['户外运动', '田园风光', '健康养生'],
    distance: '15km',
    duration: '一天',
    difficulty: '中等',
    spots: ['竹林步道', '溪谷桥', '观景亭', '茶园'],
  },
  {
    index: 4,
    title: '美食线',
    subtitle: 'Food Trail',
    description: '品味地道农家菜，探访特色小吃摊，寻找舌尖上的社村，味蕾的极致享受。',
    images: ['/images/图4.png'],
    color: 'bg-food/10',
    bgColor: 'bg-food',
    borderColor: 'border-food/30',
    textColor: 'text-food',
    icon: 'utensils',
    tags: ['农家菜', '特色小吃', '美食地图'],
    distance: '5km',
    duration: '半天',
    difficulty: '简单',
    spots: ['老街小吃', '农家宴', '手工豆腐坊', '茶庄'],
  },
]

const iconMap = {
  mountain: Mountain,
  bike: Bike,
  utensils: UtensilsCrossed,
}

const activeRoute = ref<number>(2)
const favorites = ref<Set<number>>(new Set())
const selectedRoute = ref<RouteItem | null>(null)
const imageIndexes = ref<Record<number, number>>({ 2: 1 })

function toggleFavorite(index: number) {
  const next = new Set(favorites.value)
  if (next.has(index)) next.delete(index)
  else next.add(index)
  favorites.value = next
}

function openRoute(route: RouteItem) {
  activeRoute.value = route.index
  selectedRoute.value = route
}

function closeDetail() {
  selectedRoute.value = null
}

function getIconComponent(icon: string) {
  return iconMap[icon as keyof typeof iconMap]
}

function prevImage(index: number) {
  const route = routes.find(r => r.index === index)
  if (!route || route.images.length <= 1) return
  imageIndexes.value[index] = (imageIndexes.value[index] || 0) - 1
  if (imageIndexes.value[index] < 0) imageIndexes.value[index] = route.images.length - 1
}

function nextImage(index: number) {
  const route = routes.find(r => r.index === index)
  if (!route || route.images.length <= 1) return
  imageIndexes.value[index] = (imageIndexes.value[index] || 0) + 1
  if (imageIndexes.value[index] >= route.images.length) imageIndexes.value[index] = 0
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
      <div
        v-for="route in routes"
        :key="route.index"
        :class="[
          'group relative flex-1 min-w-[260px] bg-card rounded-xl border overflow-hidden cursor-pointer transition-all duration-300 hover:-translate-y-1',
          activeRoute === route.index ? `${route.borderColor} shadow-md` : 'border-border hover:shadow-lg'
        ]"
        @click="openRoute(route)"
        @keydown.enter="openRoute(route)"
        tabindex="0"
        role="button"
        :aria-label="`查看${route.title}详情`"
      >
        <!-- Color accent bar -->
        <div :class="`h-1 w-full ${route.bgColor}`" />

        <!-- Image area -->
        <div class="relative h-40 overflow-hidden">
          <!-- Image carousel -->
          <div class="relative h-40 overflow-hidden">
            <img
              :src="route.images[imageIndexes[route.index] || 0]"
              :alt="route.title"
              class="absolute inset-0 w-full h-full object-cover transition-opacity duration-300"
              @click.stop
            />
            <div class="absolute inset-0 bg-gradient-to-t from-card via-card/40 to-transparent" />

            <!-- Prev/Next arrows (only show if multiple images) -->
            <template v-if="route.images.length > 1">
              <button
                class="absolute left-2 top-1/2 -translate-y-1/2 w-7 h-7 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
                @click.stop="prevImage(route.index)"
                aria-label="上一张"
              >
                <ChevronLeft class="w-4 h-4" />
              </button>
              <button
                class="absolute right-2 top-1/2 -translate-y-1/2 w-7 h-7 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
                @click.stop="nextImage(route.index)"
                aria-label="下一张"
              >
                <ChevronRight class="w-4 h-4" />
              </button>
              <!-- Dots indicator -->
              <div class="absolute bottom-16 left-1/2 -translate-x-1/2 flex gap-1.5 z-10">
                <span
                  v-for="(_, i) in route.images"
                  :key="i"
                  :class="[
                    'block rounded-full transition-all shadow-sm',
                    i === (imageIndexes[route.index] || 0)
                      ? 'w-2 h-2 bg-white'
                      : 'w-1.5 h-1.5 bg-white/60'
                  ]"
                />
              </div>
            </template>
          </div>

          <!-- Route number badge -->
          <div :class="`absolute top-3 left-3 w-8 h-8 rounded-lg ${route.bgColor} flex items-center justify-center font-bold text-white text-sm shadow-sm`">
            {{ route.index }}
          </div>
          <!-- Favorite button -->
          <button
            class="absolute top-3 right-3 w-8 h-8 rounded-full bg-card/80 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors"
            @click.stop="toggleFavorite(route.index)"
            :aria-label="favorites.has(route.index) ? '取消收藏' : '收藏'"
          >
            <Heart :class="`w-4 h-4 transition-colors ${favorites.has(route.index) ? 'text-food fill-food' : 'text-muted-foreground hover:text-food'}`" />
          </button>
          <!-- Icon watermark -->
          <div class="absolute inset-0 flex items-center justify-center opacity-10 pointer-events-none">
            <component :is="getIconComponent(route.icon)" class="w-20 h-20" />
          </div>
          <!-- Title overlay -->
          <div class="absolute bottom-0 left-0 right-0 p-4 bg-gradient-to-t from-card via-card/80 to-transparent">
            <h3 class="text-lg font-bold tracking-wide">{{ route.title }}</h3>
          </div>
        </div>

        <!-- Content -->
        <div class="p-4">
          <p class="text-sm text-muted-foreground leading-relaxed mb-3">
            {{ route.description }}
          </p>

          <!-- Tags -->
          <div class="flex flex-wrap gap-2 mb-3">
            <span
              v-for="(tag, i) in route.tags"
              :key="i"
              :class="`inline-flex items-center px-2 py-0.5 rounded-md text-xs font-medium ${route.color} ${route.textColor}`"
            >
              {{ tag }}
            </span>
          </div>

          <!-- Quick stats -->
          <div class="flex items-center gap-4 text-xs text-muted-foreground mb-3">
            <span class="flex items-center gap-1">
              <MapPin class="w-3 h-3" />
              {{ route.distance }}
            </span>
            <span>{{ route.duration }}</span>
            <span>{{ route.difficulty }}</span>
          </div>

          <!-- Action link -->
          <button
            :class="`inline-flex items-center gap-1.5 text-sm font-medium ${route.textColor} hover:underline group/link`"
            @click.stop="openRoute(route)"
          >
            查看详情
          </button>
        </div>
      </div>
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
        <!-- Modal header with image carousel -->
        <div class="relative h-48 overflow-hidden">
          <img
            :src="selectedRoute.images[imageIndexes[selectedRoute.index] || 0]"
            :alt="selectedRoute.title"
            class="absolute inset-0 w-full h-full object-cover"
          />
          <div class="absolute inset-0 bg-gradient-to-t from-card via-card/40 to-transparent" />
          <!-- Arrows in modal too -->
          <template v-if="selectedRoute.images.length > 1">
            <button
              class="absolute left-3 top-1/2 -translate-y-1/2 w-8 h-8 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
              @click.stop="prevImage(selectedRoute.index)"
              aria-label="上一张"
            >
              <ChevronLeft class="w-5 h-5" />
            </button>
            <button
              class="absolute right-3 top-1/2 -translate-y-1/2 w-8 h-8 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
              @click.stop="nextImage(selectedRoute.index)"
              aria-label="下一张"
            >
              <ChevronRight class="w-5 h-5" />
            </button>
          </template>
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

          <!-- Stats grid -->
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

          <!-- Spots -->
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

          <!-- Tags -->
          <div class="flex flex-wrap gap-2 mb-6">
            <span
              v-for="(tag, i) in selectedRoute.tags"
              :key="i"
              :class="`inline-flex items-center px-3 py-1 rounded-md text-xs font-medium ${selectedRoute.color} ${selectedRoute.textColor}`"
            >
              {{ tag }}
            </span>
          </div>

          <!-- Actions -->
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
