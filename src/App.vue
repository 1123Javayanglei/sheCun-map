<script setup lang="ts">
import HeroSection from './components/HeroSection.vue'
import RoutesSection from './components/RoutesSection.vue'
</script>

<template>
  <div class="min-h-screen bg-background text-foreground">
    <!-- Header -->
    <header class="border-b border-border bg-card/80 backdrop-blur-sm sticky top-0 z-40">
      <div class="max-w-5xl mx-auto px-4 py-3 flex items-center justify-between">
        <div class="flex items-center gap-2">
          <div class="w-8 h-8 rounded-lg bg-primary flex items-center justify-center">
            <svg class="w-5 h-5 text-white" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z" />
              <circle cx="12" cy="10" r="3" />
            </svg>
          </div>
          <span class="font-bold text-lg">社村地图</span>
        </div>
        <nav class="flex items-center gap-4 text-sm text-muted-foreground">
          <a href="#" class="hover:text-foreground transition-colors">首页</a>
          <a href="#" class="hover:text-foreground transition-colors">景点</a>
          <a href="#" class="text-primary font-medium">路线</a>
          <a href="#" class="hover:text-foreground transition-colors">美食</a>
        </nav>
      </div>
    </header>

    <!-- Main content -->
    <main class="max-w-5xl mx-auto px-4 py-8">
      <HeroSection />
      <RoutesSection />
    </main>

    <!-- Footer -->
    <footer class="border-t border-border mt-16 py-8">
      <div class="max-w-5xl mx-auto px-4 text-center text-sm text-muted-foreground">
        <p>社村地图 © 2026 — 发现美丽乡村，品味自然风光</p>
      </div>
    </footer>
  </div>
</template>
