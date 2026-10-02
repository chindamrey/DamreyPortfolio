<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount } from 'vue'

/* ------------------------------------------------------------------
   Props – change the content from the parent, no edits needed here
------------------------------------------------------------------- */
const props = defineProps({
  name: { type: String, default: 'Damrey' },
  photo: { type: String, default: 'images/damrey.png' }, // put your cut-out photo in /public
  roles: { type: Array, default: () => ['Full Stack Developer', 'Backend Developer', 'Frontend Developer'] },
  resumeUrl: { type: String, default: '/resume.pdf' },
  socials: {
    type: Array,
    default: () => [
      { id: 'github', label: 'GitHub', href: 'https://github.com/chindamrey' },
      { id: 'linkedin', label: 'LinkedIn', href:'https://www.linkedin.com/in/chin-damrey-21b8b1267'  },
      { id: 'facebook', label: 'Facebook', href: 'https://web.facebook.com/8s97mmvb5c'},
    ],
  },
})

/* ------------------------------------------------------------------
   Navigation
------------------------------------------------------------------- */
const links = ['Home', 'About', 'Services', 'Skills', 'Projects', 'Education']
const activeLink = ref('Home')
const menuOpen = ref(false)

function selectLink(link) {
  activeLink.value = link
  menuOpen.value = false
}

/* ------------------------------------------------------------------
   Typing effect for the role line
------------------------------------------------------------------- */
const typed = ref('')
let roleIndex = 0
let charIndex = 0
let deleting = false
let typingTimer = null

function tick() {
  const word = props.roles[roleIndex]
  if (!deleting) {
    charIndex++
    typed.value = word.slice(0, charIndex)
    if (charIndex === word.length) {
      deleting = true
      typingTimer = setTimeout(tick, 1700)
      return
    }
  } else {
    charIndex--
    typed.value = word.slice(0, charIndex)
    if (charIndex === 0) {
      deleting = false
      roleIndex = (roleIndex + 1) % props.roles.length
    }
  }
  typingTimer = setTimeout(tick, deleting ? 45 : 95)
}

/* ------------------------------------------------------------------
   Code snippet: lines light up under the cursor
------------------------------------------------------------------- */
const codeLines = [
  'const developer = {',
  "  name: 'Chin Damrey',",
  "  role: 'Web Developer',",
  "  passion: 'Building amazing",
  "  web experiences',",
  "  skills: ['HTML', 'CSS', 'JS',",
  "  'Vue.js', 'Node.js', 'MySql']",
  '};',
  '',
  'function createMagic() {',
  "  return 'Clean Code +",
  "  Creative Design';",
  '}',
  '',
  'createMagic();',
]
const activeLine = ref(-1)

/* ------------------------------------------------------------------
   Photo: 3D tilt that follows the cursor
------------------------------------------------------------------- */
const tilt = reactive({ x: 0, y: 0 })
const photoFailed = ref(false)

const photoStyle = computed(() => ({
  transform: `perspective(900px) rotateY(${tilt.x * 10}deg) rotateX(${tilt.y * -10}deg)`,
}))
const glowStyle = computed(() => ({
  transform: `translate(${tilt.x * 36}px, ${tilt.y * 36}px)`,
}))

function onPhotoMove(e) {
  const r = e.currentTarget.getBoundingClientRect()
  tilt.x = (e.clientX - r.left) / r.width - 0.5
  tilt.y = (e.clientY - r.top) / r.height - 0.5
}
function onPhotoLeave() {
  tilt.x = 0
  tilt.y = 0
}

/* ------------------------------------------------------------------
   Living background: particles + cursor spotlight
------------------------------------------------------------------- */
const heroEl = ref(null)
const canvasEl = ref(null)
const isActive = ref(false)

const pointer = { x: 0, y: 0, active: false } // plain object on purpose: updated every frame
const smooth = { x: 0, y: 0 }
const particles = []
const ripples = []
let ctx = null
let W = 0
let H = 0
let raf = 0
let reduceMotion = false

const LINK_DIST = 120
const REACH = 190
const PUSH_RADIUS = 150
const ORANGE = '255,90,40'
const WHITE = '235,235,235'

function resizeCanvas() {
  const hero = heroEl.value
  const canvas = canvasEl.value
  if (!hero || !canvas) return
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  W = hero.clientWidth
  H = hero.clientHeight
  canvas.width = W * dpr
  canvas.height = H * dpr
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0)

  const count = Math.min(90, Math.max(30, Math.round((W * H) / 14000)))
  particles.length = 0
  for (let i = 0; i < count; i++) {
    particles.push({
      x: Math.random() * W,
      y: Math.random() * H,
      vx: 0,
      vy: 0,
      r: 1 + Math.random() * 1.4,
    })
  }
  if (reduceMotion) draw(0)
}

function onMove(e) {
  const r = heroEl.value.getBoundingClientRect()

  pointer.x = e.clientX - r.left
  pointer.y = e.clientY - r.top
  if (!pointer.active) {
    smooth.x = pointer.x
    smooth.y = pointer.y
  }
  pointer.active = true
  isActive.value = true
}
function onLeave() {
  pointer.active = false
  isActive.value = false
}
function onDown(e) {
  onMove(e)
  ripples.push({ x: pointer.x, y: pointer.y, r: 0, life: 1 })
  for (const p of particles) {
    const dx = p.x - pointer.x
    const dy = p.y - pointer.y
    const d = Math.hypot(dx, dy) || 1
    if (d < 300) {
      const f = (1 - d / 300) * 9
      p.vx += (dx / d) * f
      p.vy += (dy / d) * f
    }
  }
}

function draw(t) {
  // ease the cursor so the spotlight feels weighty
  smooth.x += (pointer.x - smooth.x) * 0.14
  smooth.y += (pointer.y - smooth.y) * 0.14
  heroEl.value.style.setProperty('--sx', smooth.x.toFixed(1) + 'px')
  heroEl.value.style.setProperty('--sy', smooth.y.toFixed(1) + 'px')

  ctx.clearRect(0, 0, W, H)

  if (!reduceMotion) {
    for (const p of particles) {
      // gentle flow field
      const a = Math.sin(p.x * 0.002 + t * 0.00035) * Math.cos(p.y * 0.0022 - t * 0.0003) * Math.PI * 2
      p.vx += Math.cos(a) * 0.014
      p.vy += Math.sin(a) * 0.014

      // cursor pushes particles away, with a little swirl
      if (pointer.active) {
        const dx = p.x - smooth.x
        const dy = p.y - smooth.y
        const d = Math.hypot(dx, dy)
        if (d < PUSH_RADIUS && d > 0.1) {
          const f = 1 - d / PUSH_RADIUS
          p.vx += (dx / d) * f * 0.9 + (-dy / d) * f * 0.25
          p.vy += (dy / d) * f * 0.9 + (dx / d) * f * 0.25
        }
      }

      p.vx *= 0.965
      p.vy *= 0.965
      const sp = Math.hypot(p.vx, p.vy)
      if (sp > 7) {
        p.vx = (p.vx / sp) * 7
        p.vy = (p.vy / sp) * 7
      }
      p.x += p.vx
      p.y += p.vy
      if (p.x < -10) p.x = W + 10
      else if (p.x > W + 10) p.x = -10
      if (p.y < -10) p.y = H + 10
      else if (p.y > H + 10) p.y = -10
    }
  }

  // lines between close particles
  ctx.lineWidth = 1
  for (let i = 0; i < particles.length; i++) {
    const p = particles[i]
    for (let j = i + 1; j < particles.length; j++) {
      const q = particles[j]
      const d = Math.hypot(p.x - q.x, p.y - q.y)
      if (d < LINK_DIST) {
        ctx.strokeStyle = `rgba(${WHITE},${((1 - d / LINK_DIST) * 0.12).toFixed(3)})`
        ctx.beginPath()
        ctx.moveTo(p.x, p.y)
        ctx.lineTo(q.x, q.y)
        ctx.stroke()
      }
    }
  }

  // lines from the cursor to nearby particles
  if (pointer.active) {
    ctx.lineWidth = 1.2
    for (const p of particles) {
      const d = Math.hypot(p.x - smooth.x, p.y - smooth.y)
      if (d < REACH) {
        ctx.strokeStyle = `rgba(${ORANGE},${((1 - d / REACH) * 0.75).toFixed(3)})`
        ctx.beginPath()
        ctx.moveTo(smooth.x, smooth.y)
        ctx.lineTo(p.x, p.y)
        ctx.stroke()
      }
    }
  }

  // dots (turn orange and grow near the cursor)
  for (const p of particles) {
    let near = 0
    if (pointer.active) near = Math.max(0, 1 - Math.hypot(p.x - smooth.x, p.y - smooth.y) / REACH)
    ctx.fillStyle = near > 0
      ? `rgba(${ORANGE},${(0.6 + near * 0.4).toFixed(2)})`
      : `rgba(${WHITE},0.35)`
    ctx.beginPath()
    ctx.arc(p.x, p.y, p.r + near * 1.8, 0, Math.PI * 2)
    ctx.fill()
  }

  // click ripples
  for (let i = ripples.length - 1; i >= 0; i--) {
    const rp = ripples[i]
    rp.r += 7
    rp.life -= 0.018
    if (rp.life <= 0) {
      ripples.splice(i, 1)
      continue
    }
    ctx.strokeStyle = `rgba(${ORANGE},${(rp.life * 0.8).toFixed(3)})`
    ctx.lineWidth = 2
    ctx.beginPath()
    ctx.arc(rp.x, rp.y, rp.r, 0, Math.PI * 2)
    ctx.stroke()
  }
}

function loop(t) {
  draw(t)
  raf = requestAnimationFrame(loop)
}

/* ------------------------------------------------------------------
   Lifecycle
------------------------------------------------------------------- */
onMounted(() => {
  reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  ctx = canvasEl.value.getContext('2d')
  resizeCanvas()
  window.addEventListener('resize', resizeCanvas)

  if (reduceMotion) {
    typed.value = props.roles[0]
  } else {
    typingTimer = setTimeout(tick, 600)
    raf = requestAnimationFrame(loop)
  }
})

onBeforeUnmount(() => {
  cancelAnimationFrame(raf)
  clearTimeout(typingTimer)
  window.removeEventListener('resize', resizeCanvas)
})
</script>

<template>
  <div class="page">
    <!-- ============ Header ============
    <header class="nav">
      <div class="container nav__row">
        <a href="#home" class="brand" @click="selectLink('Home')">
          <span class="brand__mark">D</span>
          <span class="brand__name">Damrey <em>DEV</em></span>
        </a>

        <nav id="main-menu" class="menu" :class="{ 'menu--open': menuOpen }" aria-label="Main">
          <a
            v-for="link in links"
            :key="link"
            :href="`#${link.toLowerCase()}`"
            :class="{ 'is-current': activeLink === link }"
            @click="selectLink(link)"
          >{{ link }}</a>
        </nav>

        <a href="#contact" class="talk">
          Let's talk
          <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M22 2 11 13M22 2l-7 20-4-9-9-4 20-7z" /></svg>
        </a>

        <button
          class="burger"
          type="button"
          :aria-expanded="menuOpen"
          aria-controls="main-menu"
          aria-label="Toggle menu"
          @click="menuOpen = !menuOpen"
        >
          <span /><span /><span />
        </button>
      </div>
    </header> -->

    <!-- ============ Hero ============ -->
    <section id="home" ref="heroEl" class="hero" @pointermove="onMove" @pointerleave="onLeave">
      <canvas ref="canvasEl" class="field" aria-hidden="true" />
      <div class="spot" :class="{ 'spot--on': isActive }" aria-hidden="true" />

      <div class="container hero__grid">
        <!-- code snippet -->
        <div class="code" aria-hidden="true" @mouseleave="activeLine = -1">
          <div v-for="(line, i) in codeLines" :key="i" class="code__line"
            :class="{ 'is-lit': Math.abs(activeLine - i) <= 1 && activeLine >= 0, 'is-core': activeLine === i }"
            @mouseenter="activeLine = i">{{ line || ' ' }}</div>
        </div>

        <!-- intro -->
        <div class="intro">
          <p class="hello"><span class="wave">👋</span> Hello, I'm</p>

          <h1 class="name" :aria-label="props.name">
            <span v-for="(c, i) in props.name.split('')" :key="i" class="ch" aria-hidden="true">{{ c }}</span>
          </h1>

          <p class="role" aria-live="off">
            <span>{{ typed }}</span><i class="caret" />
          </p>

          <p class="bio">
            I build fast, responsive and accessible web experiences with clean code and a passion for pixel perfection.
          </p>

          <div class="actions">
            <a href="#projects" class="btn btn--primary">
              View my project
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="M5 12h14M13 6l6 6-6 6" />
              </svg>
            </a>
            <a :href="props.resumeUrl" class="btn btn--ghost" download>
              Download resume
              <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="M12 3v12m0 0-4-4m4 4 4-4M5 21h14" />
              </svg>
            </a>
          </div>

          <div class="social">
            <span>Find me on</span>
            <a v-for="s in props.socials" :key="s.id" :href="s.href" :aria-label="s.label" target="_blank"
              rel="noopener" class="social__btn">
              <svg v-if="s.id === 'github'" viewBox="0 0 16 16" aria-hidden="true">
                <path
                  d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z" />
              </svg>
              <svg v-else-if="s.id === 'linkedin'" viewBox="0 0 24 24" aria-hidden="true">
                <path
                  d="M4.98 3.5a2.5 2.5 0 1 1-.001 5.001A2.5 2.5 0 0 1 4.98 3.5zM3 9.75h4V21H3V9.75zM9.5 9.75h3.8v1.54h.05c.53-1 1.82-2.05 3.75-2.05 4 0 4.75 2.63 4.75 6.05V21h-4v-5.1c0-1.22-.02-2.78-1.7-2.78-1.7 0-1.96 1.33-1.96 2.69V21h-4V9.75z" />
              </svg>
              <svg v-else viewBox="0 0 24 24" aria-hidden="true">
                <path
                  d="M13.5 22v-8.2h2.8l.5-3.3h-3.3V8.4c0-.95.4-1.7 1.8-1.7h1.6V3.8c-.3 0-1.2-.2-2.3-.2-2.6 0-4.3 1.6-4.3 4.4v2.5H7.5v3.3h2.8V22h3.2z" />
              </svg>
            </a>
          </div>
        </div>

        <!-- photo -->
        <div class="photo" @pointermove="onPhotoMove" @pointerleave="onPhotoLeave">
          <div class="photo__glow" :style="glowStyle" aria-hidden="true" />
          <div class="photo__frame" :style="photoStyle">
            <img v-if="!photoFailed" :src="props.photo" :alt="`Portrait of ${props.name}`" @error="photoFailed = true">
            <div v-else class="photo__fallback" aria-hidden="true">D</div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Space+Grotesk:wght@500;700&family=Space+Mono:wght@400;700&display=swap');

.page {
  --bg: #0a0a0a;
  --line: #1f1f1f;
  --ink: #ffffff;
  --muted: #a3a3a3;
  --accent: #ff5a28;
  --accent-rgb: 255, 90, 40;
  --code: #41574b;
  --display: 'Space Grotesk', 'Inter', system-ui, sans-serif;
  --body: 'Inter', system-ui, sans-serif;
  --mono: 'Space Mono', ui-monospace, 'SFMono-Regular', Menlo, monospace;

  min-height: 100vh;
  background: var(--bg);
  color: var(--ink);
  font-family: var(--body);
  display: flex;
  flex-direction: column;
}

.page *,
.page *::before,
.page *::after {
  box-sizing: border-box;
}

.page svg {
  width: 1em;
  height: 1em;
  fill: currentColor;
  flex: none;
}

.container {
  width: min(1400px, 100% - 48px);
  margin-inline: auto;
}

/* ---------- header ---------- */
.nav {
  position: relative;
  z-index: 20;
  border-bottom: 1px solid var(--line);
  background: var(--bg);
}

.nav__row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 92px;
  gap: 24px;
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
  text-decoration: none;
  color: var(--ink);
}

.brand__mark {
  display: grid;
  place-items: center;
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background: var(--accent);
  font: 700 1.15rem var(--display);
}

.brand__name {
  font: 700 1.2rem var(--display);
}

.brand__name em {
  font-style: normal;
  color: var(--accent);
}

.menu {
  display: flex;
  gap: 38px;
}

.menu a {
  position: relative;
  color: #e5e5e5;
  text-decoration: none;
  font-weight: 500;
  font-size: 1.05rem;
  padding: 6px 0;
  transition: color .2s;
}

.menu a::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: -2px;
  height: 2px;
  background: var(--accent);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform .25s ease;
}

.menu a:hover {
  color: var(--accent);
}

.menu a:hover::after,
.menu a.is-current::after {
  transform: scaleX(1);
}

.talk {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  padding: 14px 24px;
  border: 1px solid var(--line);
  border-radius: 8px;
  color: var(--ink);
  text-decoration: none;
  font-weight: 600;
  font-size: 1.05rem;
  transition: border-color .2s, background .2s;
}

.talk svg {
  fill: none;
  stroke: currentColor;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
  transition: transform .25s;
}

.talk:hover {
  border-color: var(--accent);
  background: rgba(var(--accent-rgb), .08);

}

.talk:hover svg {
  transform: translate(3px, -3px);
}

.burger {
  display: none;
  background: none;
  border: 1px solid var(--line);
  border-radius: 8px;
  width: 44px;
  height: 44px;
  cursor: pointer;
  padding: 0;
}

.burger span {
  display: block;
  width: 18px;
  height: 2px;
  background: var(--ink);
  margin: 4px auto;
}

/* ---------- hero ---------- */
.hero {
  position: relative;
  flex: 1;
  overflow: hidden;
  isolation: isolate;
  display: flex;
  align-items: center;
  padding: 56px 0;
}

.field {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: -2;
}

.spot {
  position: absolute;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  opacity: 0;
  transition: opacity .5s;
  background:
    radial-gradient(circle 360px at var(--sx, 50%) var(--sy, 50%), rgba(var(--accent-rgb), .16), transparent 70%),
    radial-gradient(circle 80px at var(--sx, 50%) var(--sy, 50%), rgba(var(--accent-rgb), .16), transparent 100%);
}

.spot--on {
  opacity: 1;
}

.hero__grid {
  display: grid;
  grid-template-columns: minmax(0, .9fr) minmax(0, 1.35fr) minmax(0, 1fr);
  align-items: center;
  gap: 40px;
}

/* code */
.code {
  font: 400 .8rem/1.9 var(--mono);
  color: var(--code);
  user-select: none;
  align-self: center;
}

.code__line {
  white-space: pre;
  padding-left: 8px;
  border-left: 2px solid transparent;
  transition: color .25s, border-color .25s, transform .25s;
}

.code__line.is-lit {
  color: #8a5a47;
}

.code__line.is-core {
  color: var(--accent);
  border-left-color: var(--accent);
  transform: translateX(6px);
}

/* intro */
.hello {
  margin: 0;
  font-size: 1.35rem;
  font-weight: 500;
}

.wave {
  display: inline-block;
  transform-origin: 70% 70%;
}

.hello:hover .wave {
  animation: wave .9s ease-in-out;
}

@keyframes wave {

  0%,
  100% {
    transform: rotate(0)
  }

  20% {
    transform: rotate(18deg)
  }

  40% {
    transform: rotate(-10deg)
  }

  60% {
    transform: rotate(16deg)
  }

  80% {
    transform: rotate(-6deg)
  }
}

.name {
  margin: 6px 0 0;
  font: 700 clamp(3.6rem, 8vw, 6.2rem)/1.05 var(--display);
  letter-spacing: -.02em;
}

.ch {
  display: inline-block;
  transition: transform .25s cubic-bezier(.3, 1.6, .5, 1), color .2s;
}

.ch:hover {
  color: var(--accent);
  transform: translateY(-10px) rotate(-4deg);
}

.role {
  margin: 18px 0 0;
  font: 700 1.3rem var(--mono);
  letter-spacing: .14em;
  text-transform: uppercase;
  color: #d4d4d4;
  min-height: 1.6em;
}

.caret {
  display: inline-block;
  width: 3px;
  height: 1.15em;
  margin-left: 4px;
  background: var(--accent);
  vertical-align: -.2em;
  animation: blink 1s steps(1) infinite;
}

@keyframes blink {
  50% {
    opacity: 0;
  }
}

.bio {
  margin: 26px 0 0;
  max-width: 34ch;
  font-size: 1.28rem;
  line-height: 1.55;
  color: var(--muted);
}

.actions {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 38px;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  padding: 10px 30px;
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
  text-decoration: none;
  transition: transform .2s, box-shadow .25s, background .2s, border-color .2s;
}

.btn svg {
  fill: none;
  stroke: currentColor;
  stroke-width: 2.2;
  stroke-linecap: round;
  stroke-linejoin: round;
  transition: transform .25s;
}

.btn--primary {
  background: var(--accent);
  color: #fff;
}

.btn--primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 36px rgba(var(--accent-rgb), .45);
}

.btn--primary:hover svg {
  transform: translateX(5px);
}

.btn--ghost {
  color: var(--ink);
  border: 1px solid var(--line);
}

.btn--ghost:hover {
  border-color: var(--accent);
  background: rgba(var(--accent-rgb), .08);
}

.btn--ghost:hover svg {
  transform: translateY(3px);
}

.social {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-top: 38px;
  color: #7a7a7a;
  font-size: 1.05rem;
}

.social__btn {
  display: grid;
  place-items: center;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1px solid var(--line);
  color: var(--ink);
  font-size: 1.15rem;
  transition: transform .25s, background .2s, border-color .2s;
}

.social__btn:hover {
  background: var(--accent);
  border-color: var(--accent);
  transform: translateY(-4px) scale(1.08);
}

/* photo */
.photo {
  position: relative;
  justify-self: end;
  width: min(100%, 380px);
}

.photo__glow {
  position: absolute;
  inset: 8% -10% -6%;
  border-radius: 50%;
  z-index: 0;
  pointer-events: none;
  background: radial-gradient(closest-side, rgba(var(--accent-rgb), .42), transparent 75%);
  filter: blur(30px);
  transition: transform .35s ease-out;
}

.photo__frame {
  position: relative;
  z-index: 1;
  border-radius: 14px;
  overflow: hidden;
  transition: transform .25s ease-out;
  will-change: transform;
}

.photo__frame img {
  display: block;
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
}

.photo__fallback {
  display: grid;
  place-items: center;
  aspect-ratio: 4 / 5;
  background: #151515;
  font: 700 7rem var(--display);
  color: var(--accent);
}

/* focus */
.page a:focus-visible,
.page button:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}

/* ---------- responsive ---------- */
@media (max-width: 1100px) {
  .hero__grid {
    grid-template-columns: minmax(0, 1.2fr) minmax(0, 1fr);
  }

  .code {
    display: none;
  }
}

@media (max-width: 860px) {
  .burger {
    display: block;
  }

  .talk {
    display: none;
  }

  .menu {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    flex-direction: column;
    gap: 0;
    background: var(--bg);
    border-bottom: 1px solid var(--line);
    padding: 8px 24px 16px;
    display: none;
  }

  .menu--open {
    display: flex;
  }

  .menu a {
    padding: 14px 0;
  }

  .hero__grid {
    grid-template-columns: 1fr;
    gap: 48px;
  }

  .photo {
    justify-self: center;
    width: min(100%, 320px);
  }

  .btn {
    padding: 16px 24px;
    font-size: 1.02rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .caret {
    animation: none;
  }

  .ch,
  .photo__frame,
  .photo__glow,
  .btn,
  .code__line {
    transition: none;
  }
}
</style>
