<script setup>
import { ref } from 'vue'

const navLinks = [
  { label: 'Home', href: '#home' },
  { label: 'About', href: '#about' },
  { label: 'Services', href: '#services' },
  { label: 'Skills', href: '#skills' },
  { label: 'Projects', href: '#projects' },
  { label: 'Education', href: '#education' }
]

const menuOpen = ref(false)
</script>

<template>
  <header class="site-header">
    <div class="container header-inner">
      <a href="#home" class="logo">
        <span class="logo-mark">A</span>
        <span>Damrey <span class="accent-text">DEV</span></span>
      </a>

      <nav class="nav-desktop" aria-label="Primary">
        <a v-for="link in navLinks" :key="link.href" :href="link.href">{{ link.label }}</a>
      </nav>

      <a href="https://t.me/Damrey_chin" target="_blank" rel="noopener noreferrer" class="btn btn-outline talk-btn">
        Let's talk <i class="fa-solid fa-paper-plane"></i>
      </a>
      <button class="menu-toggle" :aria-expanded="menuOpen" aria-label="Toggle navigation menu"
        @click="menuOpen = !menuOpen">
        <i :class="menuOpen ? 'fa-solid fa-xmark' : 'fa-solid fa-bars'"></i>
      </button>
    </div>

    <nav v-if="menuOpen" class="nav-mobile" aria-label="Primary mobile">
      <a v-for="link in navLinks" :key="link.href" :href="link.href" @click="menuOpen = false">
        {{ link.label }}
      </a>
    </nav>
  </header>
</template>

<style scoped>
.site-header {
  position: sticky;
  top: 0;
  z-index: 50;
  background: rgba(10, 10, 10, 0.9);
  backdrop-filter: blur(8px);
  border-bottom: 1px solid var(--border);
}

.header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 76px;
  gap: 24px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: var(--font-display);
  font-weight: 700;
  letter-spacing: 0.02em;
  white-space: nowrap;
}

.logo-mark {
  display: grid;
  place-items: center;
  width: 32px;
  height: 32px;
  background: var(--accent);
  color: #140800;
  border-radius: 7px;
  font-size: 1rem;
}

.accent-text {
  color: var(--accent);
}

.nav-desktop {
  display: flex;
  gap: 30px;
  font-size: 0.92rem;
  font-weight: 500;
  flex: 1;
  justify-content: center;
}

.nav-desktop a {
  color: var(--text-1);
  transition: color 0.15s ease;
}

.nav-desktop a:hover {
  color: var(--text-0);
}

.talk-btn {
  padding: 10px 20px;
  font-size: 0.88rem;
}

.menu-toggle {
  display: none;
  background: none;
  color: var(--text-0);
  font-size: 1.3rem;
}

.nav-mobile {
  display: none;
}

@media (max-width: 900px) {

  .nav-desktop,
  .talk-btn {
    display: none;
  }

  .menu-toggle {
    display: block;
  }

  .nav-mobile {
    display: flex;
    flex-direction: column;
    padding: 8px 20px 20px;
    gap: 4px;
    border-top: 1px solid var(--border);
  }

  .nav-mobile a {
    padding: 12px 0;
    color: var(--text-1);
    border-bottom: 1px solid var(--border);
  }
}
</style>
