<script setup>
const stats = [
  { icon: 'fa-regular fa-face-smile', value: '50+', label: 'Happy Clients' },
  { icon: 'fa-solid fa-code', value: '80+', label: 'Projects Completed' },
  { icon: 'fa-solid fa-trophy', value: '5+', label: 'Years Experience' },
  { icon: 'fa-regular fa-star', value: '100%', label: 'Client Satisfaction' }
]
</script>


<style scoped>
.stats {
  padding: 56px 0;
  border-top: 1px solid var(--border);
  background: var(--bg-1);
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
  text-align: center;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.stat-item i {
  color: var(--accent);
  font-size: 1.3rem;
  margin-bottom: 4px;
}

.stat-item strong {
  font-family: var(--font-display);
  font-size: 1.8rem;
}

.stat-item span {
  color: var(--text-1);
  font-size: 0.85rem;
}

@media (max-width: 640px) {
  .stats-grid {
    grid-template-columns: 1fr 1fr;
  }
}
</style>
