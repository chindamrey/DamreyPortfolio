<script setup>
const details = [
  { icon: 'fa-solid fa-user', label: 'Name', value: 'Chin Damrey' },
  { icon: 'fa-solid fa-envelope', label: 'Email', value: 'chendomrey@gmail.com' },
  { icon: 'fa-solid fa-location-dot', label: 'Location', value: 'Phnum Penh, Cambodia' },
  { icon: 'fa-solid fa-globe', label: 'Nationality', value: 'Cambodian' },
]
</script>

<template>
  <section id="about" class="about">
    <div class="container about-grid">
      <div class="about-copy">
        <p class="eyebrow">About Me</p>
        <h2 class="section-heading">
          I'm a developer who loves crafting digital experiences.
        </h2>
        <p class="about-text">
          I'm a recent IT graduate passionate about
          backend development and building practical
          software solutions.

          I enjoy working with APIs, databases,
          real-time systems, and solving real-world
          problems through technology.
        </p>
        <p class="signature">Damrey</p>
      </div>

      <dl class="about-details">
        <div v-for="d in details" :key="d.label" class="detail-item">
          <div class="detail-icon"><i :class="d.icon"></i></div>
          <div>
            <dt>{{ d.label }}</dt>
            <dd>{{ d.value }}</dd>
          </div>
        </div>
      </dl>
    </div>
  </section>
</template>

<style scoped>
.about {
  padding: 72px 0;
  border-top: 1px solid var(--border);
}

.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 56px;
  align-items: start;
}

.about-copy {
  max-width: 46ch;
}

.about-text {
  color: var(--text-1);
  margin-top: 16px;
}

.signature {
  font-family: 'Space Grotesk', cursive;
  font-style: italic;
  color: var(--accent);
  font-size: 1.4rem;
  margin-top: 22px;
}

.about-details {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px 32px;
}

.detail-item {
  display: flex;
  gap: 14px;
  align-items: flex-start;
}

.detail-icon {
  display: grid;
  place-items: center;
  width: 40px;
  height: 40px;
  flex-shrink: 0;
  border-radius: 8px;
  background: var(--accent-dim);
  color: var(--accent);
}

.detail-item dt {
  font-size: 0.8rem;
  color: var(--text-2);
}

.detail-item dd {
  font-weight: 600;
  margin-top: 2px;
}

@media (max-width: 860px) {
  .about-grid {
    grid-template-columns: 1fr;
    gap: 36px;
  }

  .about-details {
    grid-template-columns: 1fr;
  }
}
</style>
