<script setup>
import { computed, onMounted, onUnmounted, ref, watch } from 'vue'

const props = defineProps({
  project: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['close'])
const selectedImageIndex = ref(0)

const projectImages = computed(() => {
  if (!props.project) return []
  return props.project.images?.length ? props.project.images : [props.project.image]
})

const selectedImage = computed(() => projectImages.value[selectedImageIndex.value])

function selectImage(index) {
  selectedImageIndex.value = index
}

function moveImage(direction) {
  const imageCount = projectImages.value.length
  if (imageCount < 2) return
  selectedImageIndex.value = (selectedImageIndex.value + direction + imageCount) % imageCount
}

function handleKeydown(e) {
  if (e.key === 'Escape') emit('close')
}

onMounted(() => document.addEventListener('keydown', handleKeydown))
onUnmounted(() => document.removeEventListener('keydown', handleKeydown))

watch(
  () => props.project,
  (val) => {
    selectedImageIndex.value = 0
    document.body.style.overflow = val ? 'hidden' : ''
  }
)
</script>

<template>
  <Teleport to="body">
    <Transition name="fade">
      <div v-if="project" class="modal-backdrop" role="dialog" aria-modal="true"
        :aria-label="project.title + ' project details'" @click.self="emit('close')">
        <div class="modal-panel">
          <button class="close-btn" aria-label="Close dialog" @click="emit('close')">
            <i class="fa-solid fa-xmark"></i>
          </button>

          <div class="modal-image">
            <img :src="selectedImage" :alt="project.title + ' preview ' + (selectedImageIndex + 1)" />
            <template v-if="projectImages.length > 1">
              <button class="image-nav image-nav-prev" aria-label="Previous project image" @click="moveImage(-1)">
                <i class="fa-solid fa-chevron-left"></i>
              </button>
              <button class="image-nav image-nav-next" aria-label="Next project image" @click="moveImage(1)">
                <i class="fa-solid fa-chevron-right"></i>
              </button>
              <div class="image-dots" aria-label="Choose project image">
                <button
                  v-for="(image, index) in projectImages"
                  :key="index"
                  class="image-dot"
                  :class="{ active: selectedImageIndex === index }"
                  :aria-label="'View project image ' + (index + 1)"
                  @click="selectImage(index)"
                ></button>
              </div>
            </template>
          </div>

          <div class="modal-body">
            <span class="modal-eyebrow">{{ project.tagline }}</span>
            <h3>{{ project.title }}</h3>
            <div v-if="project.tech && project.tech.length" class="modal-tech">
              <span>{{ project.tech }}</span>
            </div>
            <p class="modal-desc">{{ project.longDesc || project.desc }}</p>


            <ul v-if="project.highlights && project.highlights.length" class="modal-highlights">
              <li v-for="h in project.highlights" :key="h"><i class="fa-solid fa-check"></i>{{ h }}</li>
            </ul>

            <div class="modal-actions">
              <a v-if="project.liveHref" :href="project.liveHref" target="_blank" rel="noopener"
                class="btn btn-primary">
                Live site <i class="fa-solid fa-arrow-up-right-from-square"></i>
              </a>
              <a v-if="project.codeHref" :href="project.codeHref" target="_blank" rel="noopener"
                class="btn btn-outline">
                View code <i class="fa-brands fa-github"></i>
              </a>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  z-index: 100;
  background: rgba(6, 6, 6, 0.75);
  backdrop-filter: blur(3px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
}

.modal-panel {
  position: relative;
  width: min(100%, 820px);
  max-height: 88vh;
  overflow-y: auto;
  background: var(--bg-1);
  border: 1px solid var(--border);
  border-radius: 12px;
}

.close-btn {
  position: absolute;
  top: 14px;
  right: 14px;
  z-index: 2;
  width: 36px;
  height: 36px;
  border-radius: 999px;
  background: rgba(10, 10, 10, 0.7);
  color: var(--text-0);
  display: grid;
  place-items: center;
  font-size: 1rem;
}

.close-btn:hover {
  color: var(--accent);
}

.modal-image {
  position: relative;
  width: 100%;
  aspect-ratio: 3 / 2;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  background: rgba(10, 10, 10, 0.25);
}

.modal-image img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.image-nav,
.image-dot {
  position: absolute;
  display: grid;
  place-items: center;
  color: var(--text-0);
  background: rgba(10, 10, 10, 0.72);
}

.image-nav {
  top: 50%;
  width: 36px;
  height: 36px;
  border-radius: 999px;
  transform: translateY(-50%);
}

.image-nav:hover,
.image-dot:hover,
.image-dot.active {
  color: var(--accent);
}

.image-nav-prev {
  left: 14px;
}

.image-nav-next {
  right: 14px;
}

.image-dots {
  position: absolute;
  bottom: 14px;
  left: 50%;
  display: flex;
  gap: 7px;
  transform: translateX(-50%);
}

.image-dot {
  position: static;
  width: 8px;
  height: 8px;
  padding: 0;
  border: 1px solid var(--text-0);
  border-radius: 999px;
  background: transparent;
}

.image-dot.active {
  background: var(--text-0);
}

.modal-body {
  padding: 28px 28px 32px;
}

.modal-eyebrow {
  color: var(--accent);
  font-family: var(--font-mono);
  font-size: 0.78rem;
}

.modal-body h3 {
  font-family: var(--font-display);
  font-size: 1.5rem;
  margin: 8px 0 14px;
}

.modal-desc {
  color: var(--text-1);
  font-size: 0.92rem;
  margin-bottom: 18px;
}

.modal-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 18px;
}

.tech-pill {
  font-size: 0.78rem;
  padding: 5px 12px;
  border-radius: 999px;
  border: 1px solid var(--border);
  color: var(--text-1);
}

.modal-highlights {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 24px;
}

.modal-highlights li {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  font-size: 0.88rem;
  color: var(--text-1);
}

.modal-highlights i {
  color: var(--accent);
  margin-top: 3px;
}

.modal-actions {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.18s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-active .modal-panel,
.fade-leave-active .modal-panel {
  transition: transform 0.18s ease;
}

.fade-enter-from .modal-panel,
.fade-leave-to .modal-panel {
  transform: translateY(12px) scale(0.98);
}

@media (prefers-reduced-motion: reduce) {

  .fade-enter-active,
  .fade-leave-active,
  .fade-enter-active .modal-panel,
  .fade-leave-active .modal-panel {
    transition: none;
  }
}
</style>
