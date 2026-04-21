<script setup lang="ts">
import { Bike, MapPin, Heart } from 'lucide-vue-next'

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
    <div :class="`h-1 w-full ${bgColor}`" />

    <!-- Image area -->
    <div class="relative h-40 overflow-hidden">
      <img
        :src="images[0]"
        :alt="title"
        class="absolute inset-0 w-full h-full object-cover"
      />
      <div class="absolute inset-0 bg-gradient-to-t from-card via-card/40 to-transparent" />

      <!-- Route number badge -->
      <div :class="`absolute top-3 left-3 w-8 h-8 rounded-lg ${bgColor} flex items-center justify-center font-bold text-white text-sm shadow-sm`">
        {{ index }}
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
        <Bike class="w-20 h-20" />
      </div>
      <!-- Title overlay -->
      <div class="absolute bottom-0 left-0 right-0 p-4 bg-gradient-to-t from-card via-card/80 to-transparent">
        <h3 class="text-lg font-bold tracking-wide">{{ title }}</h3>
      </div>
    </div>

    <!-- Content -->
    <div class="p-4">
      <p class="text-sm text-muted-foreground leading-relaxed mb-3">{{ description }}</p>

      <div class="flex flex-wrap gap-2 mb-3">
        <span
          v-for="(tag, i) in tags"
          :key="i"
          :class="`inline-flex items-center px-2 py-0.5 rounded-md text-xs font-medium ${color} ${textColor}`"
        >
          {{ tag }}
        </span>
      </div>

      <div class="flex items-center gap-4 text-xs text-muted-foreground mb-3">
        <span class="flex items-center gap-1"><MapPin class="w-3 h-3" />{{ distance }}</span>
        <span>{{ duration }}</span>
        <span>{{ difficulty }}</span>
      </div>

      <button
        :class="`inline-flex items-center gap-1.5 text-sm font-medium ${textColor} hover:underline group/link`"
        @click.stop="emit('click')"
      >
        查看详情
      </button>
    </div>
  </div>
</template>
