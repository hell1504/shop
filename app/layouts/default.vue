<script setup lang="ts">
const cart = useCartStore();
const settings = useShopSettingsStore();

interface navLink {
  label: string;
  to: string;
  icon: string;
}

const navLinks: navLink[] = [];

const cartOpen = ref(false);
const checkoutOpen = ref(false);

function openCheckout() {
  checkoutOpen.value = true;
}
</script>

<template>
  <div class="relative min-h-screen flex flex-col bg-default overflow-x-hidden">
    <!-- Зелёное фоновое свечение (Ambient Glow) -->
    <div class="pointer-events-none fixed inset-0 z-0 overflow-hidden">
      <!-- Верхний зеленый софит -->
      <div
        class="absolute -top-40 left-1/2 -translate-x-1/2 w-[700px] sm:w-[1000px] h-[500px] bg-emerald-500/15 rounded-full blur-[130px]"
      />
      <!-- Правый фоновый акцент -->
      <div
        class="absolute top-1/3 -right-32 w-[450px] h-[450px] bg-emerald-600/10 rounded-full blur-[110px]"
      />
      <!-- Нижний левый фоновый акцент -->
      <div
        class="absolute -bottom-32 -left-32 w-[500px] h-[500px] bg-emerald-500/15 rounded-full blur-[120px]"
      />
    </div>

    <!-- Основной контент (z-10 поверх свечения) -->
    <div class="relative z-10 flex flex-col min-h-screen">
      <!-- Шапка-плашка -->
      <header class="pt-4 pb-2 w-full">
        <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
          <div
            class="flex items-center justify-between h-14 px-4 sm:px-6 rounded-2xl border border-default bg-elevated/80 backdrop-blur-xl shadow-md"
          >
            <!-- Logo + Name -->
            <NuxtLink to="/" class="flex items-center gap-3">
              <img
                src="/logo.png"
                alt="Logo"
                class="size-9 rounded-lg object-contain"
              />
              <span class="text-base font-bold tracking-tight">{{
                settings.name
              }}</span>
            </NuxtLink>

            <!-- Nav links (desktop) -->
            <nav class="hidden md:flex items-center gap-1">
              <UButton
                v-for="link in navLinks"
                :key="link.to"
                :to="link.to"
                :label="link.label"
                :icon="link.icon"
                variant="ghost"
                size="sm"
              />
            </nav>

            <!-- Right side -->
            <div class="flex items-center gap-2">
              <ShopCurrencySwitcher />
              <ClientOnly v-if="settings.cartEnabled">
                <UButton
                  variant="ghost"
                  color="neutral"
                  square
                  aria-label="Корзина"
                  @click="cartOpen = true"
                >
                  <UChip
                    :text="cart.totalItems"
                    :show="cart.totalItems > 0"
                    size="2xl"
                    color="primary"
                  >
                    <UIcon name="i-lucide-shopping-cart" class="size-5" />
                  </UChip>
                </UButton>
              </ClientOnly>
            </div>
          </div>
        </div>
      </header>

      <!-- Main -->
      <main class="flex-1 mt-2">
        <slot />
      </main>

      <!-- Cart (opt-in) -->
      <template v-if="settings.cartEnabled">
        <ShopCartDrawer v-model:open="cartOpen" @checkout="openCheckout" />
        <ShopCheckoutModal v-model:open="checkoutOpen" />
      </template>

      <!-- Footer -->
      <footer class="border-t border-default mt-8">
        <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
          <!-- Footer top -->
          <div
            class="flex flex-col gap-6 py-6 md:flex-row md:items-center md:justify-between"
          >
            <div class="flex items-center gap-3.5">
              <img
                src="/logo.png"
                alt="Logo"
                class="size-12 rounded-lg object-contain"
              />
              <div>
                <p class="font-bold">
                  {{ settings.name }}
                </p>
                <p v-if="settings.description" class="text-sm text-muted">
                  {{ settings.description }}
                </p>
              </div>
            </div>

            <nav class="flex flex-wrap gap-x-4 gap-y-1 text-sm">
              <NuxtLink to="/legal/offer" class="text-muted hover:text-default">
                Публичная оферта
              </NuxtLink>
              <NuxtLink to="/legal/terms" class="text-muted hover:text-default">
                Пользовательское соглашение
              </NuxtLink>
              <NuxtLink
                to="/legal/privacy"
                class="text-muted hover:text-default"
              >
                Политика конфиденциальности
              </NuxtLink>
            </nav>
          </div>

          <!-- Footer bottom -->
          <div
            class="border-t border-default py-4 text-center text-sm text-muted"
          >
            <p>
              &copy; {{ new Date().getFullYear() }} {{ settings.name }}. Все
              права защищены.
            </p>
            <p class="mt-1">
              Сайт создан
              <NuxtLink
                to="https://fd.zaralx.ru"
                target="_blank"
                class="text-primary hover:underline"
              >
                FreshDonate
              </NuxtLink>
            </p>
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>
