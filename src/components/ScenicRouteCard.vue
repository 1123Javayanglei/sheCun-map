<script setup lang="ts">
import { Mountain, MapPin, Heart, ChevronLeft, ChevronRight } from 'lucide-vue-next'
import { ref } from 'vue'

interface Props {
  index: number
  title: string
  subtitle: string
  description: string
  images: string[]
  color: string
  bgColor: string
  borderColor: string
  textColor: string
  tags: string[]
  distance: string
  duration: string
  difficulty: string
  isActive: boolean
  isFavorite: boolean
}

const props = defineProps<Props>()
const emit = defineEmits<{
  click: []
  favorite: []
}>()

const imageIndex = ref(0)

function prevImage() {
  if (props.images.length <= 1) return
  imageIndex.value--
  if (imageIndex.value < 0) imageIndex.value = props.images.length - 1
}

function nextImage() {
  if (props.images.length <= 1) return
  imageIndex.value++
  if (imageIndex.value >= props.images.length) imageIndex.value = 0
}
</script>

<template>
  <div
    :class="[
      'group relative flex-1 min-w-[260px] bg-card rounded-xl border overflow-hidden cursor-pointer transition-all duration-300 hover:-translate-y-1',
      isActive ? `${borderColor} shadow-md` : 'border-border hover:shadow-lg'
    ]"
    @click="emit('click')"
    @keydown.enter="emit('click')"
    tabindex="0"
    role="button"
    :aria-label="`查看${title}详情`"
  >
    <!-- Color accent bar -->
    <div :class="`h-1 w-full ${bgColor}`" />

    <!-- Image area -->
    <div class="relative h-40 overflow-hidden">
      <!-- Image carousel -->
      <div class="relative h-40 overflow-hidden">
        <img
          :src="images[imageIndex]"
          :alt="title"
          class="absolute inset-0 w-full h-full object-cover transition-opacity duration-300"
          @click.stop
        />
        <div class="absolute inset-0 bg-gradient-to-t from-card via-card/40 to-transparent" />

        <!-- Prev/Next arrows -->
        <template v-if="images.length > 1">
          <button
            class="absolute left-2 top-1/2 -translate-y-1/2 w-7 h-7 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
            @click.stop="prevImage"
            aria-label="上一张"
          >
            <ChevronLeft class="w-4 h-4" />
          </button>
          <button
            class="absolute right-2 top-1/2 -translate-y-1/2 w-7 h-7 rounded-full bg-card/70 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors z-10"
            @click.stop="nextImage"
            aria-label="下一张"
          >
            <ChevronRight class="w-4 h-4" />
          </button>
          <!-- Dots indicator -->
          <div class="absolute bottom-16 left-1/2 -translate-x-1/2 flex gap-1.5 z-10">
            <span
              v-for="(_, i) in images"
              :key="i"
              :class="[
                'block rounded-full transition-all shadow-sm',
                i === imageIndex ? 'w-2 h-2 bg-white' : 'w-1.5 h-1.5 bg-white/60'
              ]"
            />
          </div>
        </template>
      </div>

      <!-- Favorite button -->
      <button
        class="absolute top-3 right-3 w-8 h-8 rounded-full bg-card/80 backdrop-blur-sm flex items-center justify-center hover:bg-card transition-colors"
        @click.stop="emit('favorite')"
        :aria-label="isFavorite ? '取消收藏' : '收藏'"
      >
        <Heart :class="`w-4 h-4 transition-colors ${isFavorite ? 'text-food fill-food' : 'text-muted-foreground hover:text-food'}`" />
      </button>
      <!-- Icon watermark -->
      <div class="absolute inset-0 flex items-center justify-center opacity-10 pointer-events-none">
        <Mountain class="w-20 h-20" />
      </div>
      <!-- Title overlay -->
      <div class="absolute bottom-0 left-0 right-0 p-4 bg-gradient-to-t from-card via-card/80 to-transparent">
        <h3 class="text-lg font-bold tracking-wide">{{ title }}</h3>
      </div>
    </div>

    <!-- Content -->
    <div class="p-4">
      <p class="text-sm text-muted-foreground leading-relaxed mb-3">
        {{ description }}
      </p>

      <!-- Tags -->
      <div class="flex flex-wrap gap-2 mb-3">
        <span
          v-for="(tag, i) in tags"
          :key="i"
          :class="`inline-flex items-center px-2 py-0.5 rounded-md text-xs font-medium ${color} ${textColor}`"
        >
          {{ tag }}
        </span>
      </div>

      <!-- Quick stats -->
      <div class="flex items-center gap-4 text-xs text-muted-foreground mb-3">
        <span class="flex items-center gap-1">
          <MapPin class="w-3 h-3" />
          {{ distance }}
        </span>
        <span>{{ duration }}</span>
        <span>{{ difficulty }}</span>
      </div>

      <!-- Action link -->
      <button
        :class="`inline-flex items-center gap-1.5 text-sm font-medium ${textColor} hover:underline group/link`"
        @click.stop="emit('click')"
      >
        查看详情
      </button>
    </div>
  </div>
</template>
