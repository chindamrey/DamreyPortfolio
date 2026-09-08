<script setup>
const services = [
  {
    icon: 'fa-solid fa-code',
    title: 'Frontend Development',
    desc: 'Creating responsive, user-friendly, and maintainable web interfaces using Vue.js, JavaScript, HTML, CSS, and Bootstrap.'
  },

  {
    icon: 'fa-solid fa-server',
    title: 'Backend Development',
    desc: 'Developing RESTful APIs with Node.js, Express.js, MySQL, authentication, and real-time applications.'
  },

]
</script>

<template>
  <section id="services" class="services">
    <div class="container">
      <div class="services-heading">
        <p class="eyebrow">What I Do</p>
        <!-- <h2 class="section-heading">Services I Offer</h2> -->
      </div>

      <div class="services-grid">
        <article v-for="s in services" :key="s.title" class="service-card">
          <div class="service-icon"><i :class="s.icon"></i></div>
          <h3>{{ s.title }}</h3>
          <p>{{ s.desc }}</p>
        </article>
      </div>
    </div>
  </section>
</template>

<style scoped>
.services {
  padding: 72px 0;
  border-top: 1px solid var(--border);
}

.services-heading {
  text-align: center;
  margin-bottom: 44px;
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.service-card {
  background: var(--bg-1);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 28px 24px;
  transition: border-color 0.15s ease;
}

.service-card:hover {
  border-color: var(--accent);
}

.service-icon {
  display: grid;
  place-items: center;
  width: 46px;
  height: 46px;
  border-radius: 8px;
  border: 1px solid var(--accent);
  color: var(--accent);
  font-size: 1.1rem;
  margin-bottom: 20px;
}

.service-card h3 {
  font-family: var(--font-display);
  font-size: 1.05rem;
  margin-bottom: 10px;
}

.service-card p {
  color: var(--text-1);
  font-size: 0.9rem;
}

@media (max-width: 980px) {
  .services-grid {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 560px) {
  .services-grid {
    grid-template-columns: 1fr;
  }
}
</style>
