<script setup>
import { ref, computed, reactive } from 'vue'

const testimonials = [
  {
    quote: "Arjun is an exceptional developer. He delivers clean, maintainable code and ensures every detail is perfect. Highly recommended!",
    name: 'Priya Sharma',
    role: 'Product Manager, TaskFlow'
  },
  {
    quote: "Working with Arjun was seamless. He translated our designs into a fast, polished product ahead of schedule.",
    name: 'Rohan Mehta',
    role: 'Founder, Wanderlust'
  },
  {
    quote: "Great communicator and even better engineer. Our dashboard performance improved dramatically after his rebuild.",
    name: 'Neha Kapoor',
    role: 'CTO, FinTrack'
  }
]

const activeIndex = ref(0)
const active = computed(() => testimonials[activeIndex.value])

function prev() {
  activeIndex.value = (activeIndex.value - 1 + testimonials.length) % testimonials.length
}

function next() {
  activeIndex.value = (activeIndex.value + 1) % testimonials.length
}

const form = reactive({ name: '', email: '', message: '' })
const submitted = ref(false)

function handleSubmit() {
  submitted.value = true
  form.name = ''
  form.email = ''
  form.message = ''
}
</script>

<template>
  <section id="contact" class="testi-contact">
    <div class="container tc-grid">
      <div class="testimonials-block">
        <p class="eyebrow">Testimonials</p>
        <h2 class="section-heading">What Clients Say</h2>

        <div class="testimonial-card">
          <i class="fa-solid fa-quote-left quote-mark"></i>
          <p class="quote">{{ active.quote }}</p>
          <div class="testimonial-author">
            <div class="avatar">{{ active.name.charAt(0) }}</div>
            <div>
              <strong>{{ active.name }}</strong>
              <span>{{ active.role }}</span>
            </div>
          </div>
        </div>

        <div class="testimonial-nav">
          <button class="nav-btn" aria-label="Previous testimonial" @click="prev">
            <i class="fa-solid fa-chevron-left"></i>
          </button>
          <div class="dots">
            <span
              v-for="(t, i) in testimonials"
              :key="t.name"
              class="dot"
              :class="{ active: i === activeIndex }"
            ></span>
          </div>
          <button class="nav-btn" aria-label="Next testimonial" @click="next">
            <i class="fa-solid fa-chevron-right"></i>
          </button>
        </div>
      </div>

      <div class="contact-block">
        <p class="eyebrow">Let's Work Together</p>
        <h2 class="section-heading">Get In Touch</h2>
        <p class="contact-desc">Have a project in mind or just want to say hello? Feel free to reach out.</p>

        <ul class="contact-info">
          <li><i class="fa-solid fa-envelope"></i> hello@arjundev.com</li>
          <li><i class="fa-solid fa-phone"></i> +91 98765 43210</li>
          <li><i class="fa-solid fa-location-dot"></i> Bangalore, India</li>
        </ul>
      </div>

      <form class="contact-form" @submit.prevent="handleSubmit">
        <label class="sr-only" for="name">Your name</label>
        <input id="name" v-model="form.name" type="text" placeholder="Your Name" required />

        <label class="sr-only" for="email">Your email</label>
        <input id="email" v-model="form.email" type="email" placeholder="Your Email" required />

        <label class="sr-only" for="message">Your message</label>
        <textarea id="message" v-model="form.message" rows="5" placeholder="Your Message" required></textarea>

        <button type="submit" class="btn btn-primary">
          Send message <i class="fa-solid fa-paper-plane"></i>
        </button>

        <p v-if="submitted" class="sent-note" role="status">Message sent. I'll get back to you soon.</p>
      </form>
    </div>
  </section>
</template>

<style scoped>
.testi-contact {
  padding: 72px 0;
  border-top: 1px solid var(--border);
}

.tc-grid {
  display: grid;
  grid-template-columns: 1fr 1fr 0.9fr;
  gap: 40px;
  align-items: start;
}

.testimonial-card {
  background: var(--bg-1);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 28px;
  margin-top: 22px;
  position: relative;
}

.quote-mark {
  color: var(--accent);
  font-size: 1.4rem;
  margin-bottom: 12px;
}

.quote {
  color: var(--text-1);
  font-size: 0.95rem;
}

.testimonial-author {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 20px;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 999px;
  background: var(--accent-dim);
  color: var(--accent);
  display: grid;
  place-items: center;
  font-weight: 700;
  flex-shrink: 0;
}

.testimonial-author strong {
  display: block;
  font-size: 0.9rem;
}

.testimonial-author span {
  font-size: 0.78rem;
  color: var(--accent);
}

.testimonial-nav {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-top: 18px;
}

.nav-btn {
  width: 32px;
  height: 32px;
  border-radius: 999px;
  border: 1px solid var(--border);
  background: transparent;
  color: var(--text-0);
  display: grid;
  place-items: center;
}

.nav-btn:hover {
  border-color: var(--accent);
  color: var(--accent);
}

.dots {
  display: flex;
  gap: 8px;
}

.dot {
  width: 7px;
  height: 7px;
  border-radius: 999px;
  background: var(--border);
}

.dot.active {
  background: var(--accent);
}

.contact-desc {
  color: var(--text-1);
  margin: 16px 0 20px;
  max-width: 32ch;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.contact-info li {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 0.92rem;
}

.contact-info i {
  color: var(--accent);
  width: 20px;
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 14px;
  background: var(--bg-1);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 24px;
}

.contact-form input,
.contact-form textarea {
  background: var(--bg-0);
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 12px 14px;
  color: var(--text-0);
  font-family: inherit;
  font-size: 0.9rem;
  resize: vertical;
}

.contact-form input::placeholder,
.contact-form textarea::placeholder {
  color: var(--text-2);
}

.contact-form input:focus,
.contact-form textarea:focus {
  border-color: var(--accent);
}

.sent-note {
  color: var(--accent);
  font-size: 0.85rem;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0 0 0 0);
}

@media (max-width: 980px) {
  .tc-grid {
    grid-template-columns: 1fr;
  }
}
</style>
