<script setup>
import { ref } from 'vue'

const quickLinks = [
  { label: 'Home', href: '#home' },
  { label: 'About', href: '#about' },
  { label: 'Services', href: '#services' },
  { label: 'Projects', href: '#projects' },
  { label: 'Education', href: '#education' }
]

const socials = [
  { icon: 'fa-brands fa-github', href: 'https://github.com/chindamrey' },
  { icon: 'fa-brands fa-linkedin-in', href: 'https://www.linkedin.com/in/chin-damrey-21b8b1267' },
  { icon: 'fa-brands fa-facebook', href: 'https://web.facebook.com/8s97mmvb5c' }
]

const email = ref('')
const subscribed = ref(false)

function subscribe() {
  if (!email.value) return
  subscribed.value = true
  email.value = ''
}
</script>

<template>
  <footer class="site-footer">
    <div class="container footer-grid">
      <div class="footer-brand">
        <a href="#home" class="logo">
          <span class="logo-mark">D</span>
          <span>Damrey <span class="accent-text">DEV</span></span>
        </a>
        <p>Building the web, one pixel at a time.</p>
      </div>

      <div class="footer-col">
        <h4>Quick Links</h4>
        <ul>
          <li v-for="l in quickLinks" :key="l.href"><a :href="l.href">{{ l.label }}</a></li>
        </ul>
      </div>

      <div class="footer-col">
        <h4>Follow Me</h4>
        <div class="footer-socials">
          <a v-for="s in socials" :key="s.icon" :href="s.href" target="_blank" rel="noopener"><i :class="s.icon"></i></a>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.site-footer {
  border-top: 1px solid var(--border);
  padding: 56px 0 0;
  background: var(--bg-1);
}

.footer-grid {
  display: grid;
  grid-template-columns: 1.2fr 1fr 1fr;
  gap: 32px;
  padding-bottom: 36px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: var(--font-display);
  font-weight: 700;
  margin-bottom: 12px;
}

.logo-mark {
  display: grid;
  place-items: center;
  width: 30px;
  height: 30px;
  background: var(--accent);
  color: #140800;
  border-radius: 7px;
}

.accent-text {
  color: var(--accent);
}

.footer-brand p,
.footer-col p {
  color: var(--text-1);
  font-size: 0.88rem;
}

.footer-col h4 {
  font-family: var(--font-display);
  font-size: 0.95rem;
  margin-bottom: 16px;
}

.footer-col ul {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.footer-col ul a {
  color: var(--text-1);
  font-size: 0.9rem;
  transition: color 0.15s ease;
}

.footer-col ul a:hover {
  color: var(--accent);
}

.footer-socials {
  display: flex;
  gap: 10px;
  margin-bottom: 14px;
}

.footer-socials a {
  width: 34px;
  height: 34px;
  display: grid;
  place-items: center;
  border-radius: 999px;
  border: 1px solid var(--border);
}

.footer-socials a:hover {
  color: var(--accent);
  border-color: var(--accent);
}
.footer-note {
  font-size: 0.85rem;
  margin-top: 4px;
}

.subscribe-form {
  display: flex;
  gap: 8px;
  margin-top: 14px;
}

.subscribe-form input {
  flex: 1;
  background: var(--bg-0);
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 10px 12px;
  color: var(--text-0);
  font-size: 0.85rem;
}

.subscribe-form button {
  background: var(--accent);
  color: #140800;
  width: 40px;
  border-radius: 6px;
}

.footer-bottom {
  display: flex;
  justify-content: space-between;
  padding: 20px 32px;
  border-top: 1px solid var(--border);
  color: var(--text-2);
  font-size: 0.82rem;
}

.footer-bottom i {
  color: var(--accent);
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0 0 0 0);
}

@media (max-width: 900px) {
  .footer-grid {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 560px) {
  .footer-grid {
    grid-template-columns: 1fr;
  }

  .footer-bottom {
    flex-direction: column;
    gap: 6px;
    text-align: center;
  }
}
</style>
