<script setup lang="ts">
defineProps<{
  title: string;
  serverIp?: string;
}>();

const copied = ref(false);

function copyIp(ip: string) {
  navigator.clipboard.writeText(ip);
  copied.value = true;
  setTimeout(() => {
    copied.value = false;
  }, 2000);
}
</script>

<template>
  <div
    class="relative overflow-hidden rounded-2xl border border-default bg-cover bg-center shadow-lg"
    style="background-image: url(&quot;/banner.png&quot;)"
  >
    <!-- Тёмная подложка-градиент для читаемости текста поверх любой картинки -->
    <div
      class="absolute inset-0 bg-gradient-to-r from-black/80 via-black/50 to-black/20 z-0 pointer-events-none"
    />

    <div
      class="relative z-10 flex items-center justify-between px-8 py-12 md:py-16"
    >
      <div class="space-y-4">
        <h1
          class="text-3xl md:text-4xl font-extrabold tracking-tight text-white drop-shadow-md"
        >
          {{ title }}
        </h1>

        <UButton
          v-if="serverIp"
          :label="copied ? 'Скопировано!' : serverIp"
          :icon="copied ? 'i-lucide-check' : 'i-lucide-copy'"
          variant="subtle"
          color="neutral"
          size="lg"
          class="backdrop-blur-md bg-white/10 hover:bg-white/20 text-white border border-white/20"
          @click="copyIp(serverIp!)"
        />
      </div>

      <!-- Правый блок с киркой (сохранили стиль, сделали слегка прозрачным) -->
      <div class="hidden md:block">
        <div
          class="size-48 rounded-xl bg-black/30 backdrop-blur-sm border border-white/10 flex items-center justify-center"
        >
          <UIcon name="i-lucide-pickaxe" class="size-24 text-white/40" />
        </div>
      </div>
    </div>
  </div>
</template>
