<script setup>
const education = [
  {
    degree: 'Bachelor\'s Degree in Information Technology',
    school: 'National Institute of Bussiness, Phnom Penh',
    period: '2023 — 2026',
    detail: 'Focused on data structures, Software Development, Database, Web Technologies '
  },
  {
    degree: 'High School Diploma',
    school: 'Sok An Tonle Bati',
    period: '2020 — 2022',
    detail: ''
  },
 
]
const scholership = [
  {
    degree: 'Web Development Scholarship',
    school: 'Abstract of New Technology (ANT), Phnom Penh',
    period: '2025 — 2026',
    detail: 'Focused on Frontend , Backend Web Development '
  },
]
</script>

<template>
  <section id="education" class="education">
    <div class="container">
      <p class="eyebrow">Background</p>
      <h2 class="section-heading">Education</h2>

      <ul class="timeline">
        <li v-for="edu in education" :key="edu.degree" class="timeline-item">
          <div class="timeline-marker" aria-hidden="true"></div>
          <div class="timeline-content">
            <span class="period">{{ edu.period }}</span>
            <h3>{{ edu.degree }}</h3>
            <p class="school">{{ edu.school }}</p>
            <p class="detail">{{ edu.detail }}</p>
          </div>
        </li>
      </ul>
      <h2 class="section-heading">Professional Training</h2>

      <ul class="timeline">
        <li v-for="edu in scholership" :key="edu.degree" class="timeline-item">
          <div class="timeline-marker"></div>
          <div class="timeline-content">
            <span class="period">{{ edu.period }}</span>
            <h3>{{ edu.degree }}</h3>
            <p class="school">{{ edu.school }}</p>
            <p class="detail">{{ edu.detail }}</p>
          </div>
        </li>
      </ul>
    </div>
  </section>
</template>

<style scoped>
.education {
  padding: 72px 0;
  border-top: 1px solid var(--border);
}

.timeline {
  position: relative;
  margin-top: 40px;
  padding-left: 28px;
  border-left: 1px solid var(--border);
}

.timeline-item {
  position: relative;
  padding-bottom: 40px;
}

.timeline-item:last-child {
  padding-bottom: 0;
}

.timeline-marker {
  position: absolute;
  left: -33px;
  top: 4px;
  width: 12px;
  height: 12px;
  border-radius: 999px;
  background: var(--bg-0);
  border: 2px solid var(--accent);
}

.period {
  font-family: var(--font-mono);
  font-size: 0.78rem;
  color: var(--accent);
}

.timeline-content h3 {
  font-family: var(--font-display);
  font-size: 1.1rem;
  margin: 6px 0 4px;
}

.school {
  color: var(--text-1);
  font-size: 0.9rem;
  font-weight: 500;
  margin-bottom: 8px;
}

.detail {
  color: var(--text-1);
  font-size: 0.88rem;
  max-width: 56ch;
}

@media (max-width: 640px) {
  .timeline {
    padding-left: 22px;
  }

  .timeline-marker {
    left: -27px;
  }
}
</style>
