<script setup>
const stack = [
  { icon: 'fa-brands fa-html5', label: 'HTML5', color: '#e34f26' },
  { icon: 'fa-brands fa-css3-alt', label: 'CSS3', color: '#2965f1' },
  { icon: 'fa-brands fa-square-js', label: 'JavaScript', color: '#f0db4f' },
  { icon: 'fa-brands fa-node', label: 'Node.js', color: '#4d8d47' },
  { icon: 'fa-brands fa-bootstrap', label: 'Bootstrap', color: '#c85be9' },
  { icon: 'fa-brands fa-vuejs', label: 'Vue.js', color: '#4d8d47' },
  { icon: 'fa-brands fa-git-alt', label: 'Git', color: '#f05033' },
  { icon: 'fa-brands fa-github', label: 'GitHub', color: '#ffffff' }
]
</script>

<template>
  <section id="skills" class="tech">
    <div class="container">
      <p class="eyebrow center">Tech Stack</p>
      <ul class="tech-grid">
        <li v-for="t in stack" :key="t.label" class="tech-item">
          <span class="tech-icon" :style="{ color: t.color }"><i :class="t.icon"></i></span>
          <span class="tech-label">{{ t.label }}</span>
        </li>
      </ul>
    </div>
  </section>
</template>

<style scoped>
.tech {
  padding: 56px 0;
  border-top: 1px solid var(--border);
}

.center {
  text-align: center;
  display: block;
  margin-bottom: 32px;
}

.tech-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 40px 56px;
}

.tech-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.tech-icon {
  display: grid;
  place-items: center;
  width: 56px;
  height: 56px;
  border-radius: 999px;
  border: 1px solid var(--border);
  font-size: 1.5rem;
  background: var(--bg-1);
}

.tech-label {
  font-size: 0.82rem;
  color: var(--text-1);
}
</style>
