<script setup lang="ts">
import { Sparkles, MapPin, Navigation } from 'lucide-vue-next'
</script>

<template>
  <section class="relative overflow-hidden rounded-2xl mb-10 border border-border">
    <div class="relative h-[320px] md:h-[400px] overflow-hidden">
      <!-- Background image -->
      <img src="/images/图1.png" alt="佘村风景" class="absolute inset-0 w-full h-full object-cover" />
      <!-- Dark overlay for readability -->
      <div class="absolute inset-0 bg-gradient-to-t from-background/80 via-background/30 to-transparent" />
      <!-- Decorative mountain SVG -->
      <svg class="absolute bottom-0 left-0 right-0 w-full" viewBox="0 0 1200 200" fill="none" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none">
        <path d="M0 200L150 120L300 160L450 80L600 140L750 60L900 120L1050 40L1200 100V200H0Z" fill="hsl(var(--primary))" fill-opacity="0.08" />
        <path d="M0 200L200 150L400 170L600 110L800 160L1000 90L1200 130V200H0Z" fill="hsl(var(--primary))" fill-opacity="0.05" />
        <path d="M0 200L100 170L250 180L400 140L550 165L700 130L850 155L1000 120L1200 145V200H0Z" fill="hsl(var(--accent))" />
      </svg>
      <!-- Floating decorative dots -->
      <div class="absolute top-16 left-20 w-2 h-2 rounded-full bg-primary/30 animate-pulse" />
      <div class="absolute top-24 right-32 w-3 h-3 rounded-full bg-cycling/20 animate-pulse" style="animation-delay: 0.5s" />
      <div class="absolute top-32 left-1/3 w-1.5 h-1.5 rounded-full bg-food/30 animate-pulse" style="animation-delay: 1s" />

      <!-- Content overlay -->
      <div class="absolute inset-0 flex flex-col items-center justify-center text-center px-6">
        <h1 class="text-3xl md:text-5xl font-bold tracking-tight mb-3">
          <span class="text-primary">精品</span>旅游路线
        </h1>
        <p class="text-muted-foreground text-sm md:text-base max-w-md">
          探索佘村最美的风景，发现隐藏的宝藏打卡地
        </p>
        <!-- Stats -->
        <div class="flex items-center gap-6 mt-6">
          <div class="flex items-center gap-2 text-sm text-muted-foreground">
            <MapPin class="w-4 h-4 text-primary" />
            <span>3条精选路线</span>
          </div>
          <div class="flex items-center gap-2 text-sm text-muted-foreground">
            <Sparkles class="w-4 h-4 text-food" />
            <span>热门景点全覆盖</span>
          </div>
        </div>
      </div>

      <!-- Navigation indicator -->
      <div class="absolute bottom-28 left-6 md:left-10 flex items-center gap-2 text-xs text-muted-foreground">
        <Navigation class="w-3.5 h-3.5" />
        <span>佘村全域</span>
      </div>
    </div>
  </section>
</template>
