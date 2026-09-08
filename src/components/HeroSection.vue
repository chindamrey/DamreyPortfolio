<script setup>
const socials = [
  { icon: 'fa-brands fa-github', href: 'https://github.com/chindamrey' },
  { icon: 'fa-brands fa-linkedin-in', href: 'https://www.linkedin.com/in/chin-damrey-21b8b1267' },
  { icon: 'fa-brands fa-facebook', href: 'https://web.facebook.com/8s97mmvb5c' }
]

const codeLines = [
  { text: 'const developer = {', indent: 0 },
  { text: "name: 'Chin Damrey',", indent: 1 },
  { text: "role: 'Web Developer',", indent: 1 },
  { text: "passion: 'Building amazing", indent: 1 },
  { text: "web experiences',", indent: 1 },
  { text: "skills: ['HTML', 'CSS', 'JS',", indent: 1 },
  { text: "'Vue.js', 'Node.js'],", indent: 1 },
  { text: '};', indent: 0 },
  { text: '', indent: 0 },
  { text: 'function createMagic() {', indent: 0 },
  { text: "return 'Clean Code +", indent: 1 },
  { text: "Creative Design';", indent: 1 },
  { text: '}', indent: 0 },
  { text: '', indent: 0 },
  { text: 'createMagic();', indent: 0 }
]
</script>

<template>
  <section id="home" class="hero">
    <div class="container hero-grid">
      <pre class="code-panel" aria-hidden="true"><code
        ><span v-for="(line, i) in codeLines" :key="i" :style="{ paddingLeft: line.indent * 16 + 'px' }"
          >{{ line.text }}<br
        /></span></code></pre>

      <div class="hero-content">
        <p class="greeting">👋 Hello, I'm</p>
        <h1 class="hero-name">Damrey <span class="accent-text"></span></h1>
        <p class="hero-role">WEB DEVELOPER<span class="cursor">|</span></p>
        <p class="hero-desc">
          I build fast, responsive and accessible web experiences with clean code and a passion for pixel perfection.
        </p>

        <div class="hero-actions">
          <a href="#projects" class="btn btn-primary">View my project <i class="fa-solid fa-arrow-right"></i></a>
          <a href="@/assets/CV/Chin Damrey_Junior Full Stack Developer.pdf" download="Chin Damrey_Junior Full Stack Developer.pdf" class="btn btn-outline">Download resume <i class="fa-solid fa-download"></i></a>
        </div>

        <div class="hero-socials">
          <span>Find me on</span>
          <a v-for="s in socials" :key="s.icon" :href="s.href" class="social-btn" target="_blank" rel="noopener">
            <i :class="s.icon"></i>
          </a>
        </div>
      </div>

      <div class="hero-photo-wrap">
        <img
          src="../../public/images/damrey.png"
          alt="Portrait of Arjun Dev, frontend developer"
          class="hero-photo"
        />
       
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  padding: 56px 0 72px;
  overflow: hidden;
}

.hero-grid {
  display: grid;
  grid-template-columns: 0.9fr 1.4fr 1fr;
  gap: 24px;
  align-items: center;
}

.code-panel {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  line-height: 1.7;
  color: #5c8f6e;
  opacity: 0.55;
  white-space: pre;
}

.hero-content {
  min-width: 0;
}

.greeting {
  font-size: 1.1rem;
  margin-bottom: 6px;
}

.hero-name {
  font-family: var(--font-display);
  font-size: clamp(2.4rem, 5.5vw, 3.6rem);
  font-weight: 700;
  line-height: 1.05;
}

.hero-role {
  font-family: var(--font-mono);
  font-size: 1rem;
  letter-spacing: 0.1em;
  color: var(--text-1);
  margin: 14px 0 16px;
}

.cursor {
  color: var(--accent);
  animation: blink 1.1s step-end infinite;
}

@keyframes blink {
  50% {
    opacity: 0;
  }
}

.hero-desc {
  color: var(--text-1);
  max-width: 40ch;
  margin-bottom: 28px;
}

.hero-actions {
  display: flex;
  gap: 14px;
  flex-wrap: wrap;
  margin-bottom: 30px;
}

.hero-socials {
  display: flex;
  align-items: center;
  gap: 14px;
  font-size: 0.85rem;
  color: var(--text-2);
}

.social-btn {
  display: grid;
  place-items: center;
  width: 34px;
  height: 34px;
  border-radius: 999px;
  border: 1px solid var(--border);
  color: var(--text-0);
  transition: border-color 0.15s ease, color 0.15s ease;
}

.social-btn:hover {
  border-color: var(--accent);
  color: var(--accent);
}

.hero-photo-wrap {
  position: relative;
  justify-self: center;
}

.hero-photo {
  width: 100%;
  max-width: 300px;
  aspect-ratio: 3 / 4;
  object-fit: cover;
  border-radius: 10px;
  position: relative;
  z-index: 1;
}

.exp-badge {
  position: absolute;
  right: -14px;
  bottom: 26px;
  z-index: 2;
  background: var(--bg-1);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 14px 16px;
  display: flex;
  flex-direction: column;
  gap: 4px;
  min-width: 120px;
}

.exp-badge strong {
  font-family: var(--font-display);
  font-size: 1.6rem;
  color: var(--accent);
}

.exp-badge span {
  font-size: 0.68rem;
  letter-spacing: 0.05em;
  color: var(--text-1);
}

.exp-badge i {
  color: var(--accent);
  margin-top: 6px;
}

@media (max-width: 1080px) {
  .hero-grid {
    grid-template-columns: 1fr;
    text-align: left;
  }

  .code-panel {
    display: none;
  }

  .hero-photo-wrap {
    justify-self: start;
    margin-top: 20px;
  }
}
</style>
