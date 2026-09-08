<script setup>
import ProjectModal from './ProjectModal.vue'
import { ref } from 'vue';
// Importing images for the auction project
import auction1 from '@/assets/images/auctions/1.png';
import auction4 from '@/assets/images/auctions/4.png';
import auction2 from '@/assets/images/auctions/2.png';
import auction3 from '@/assets/images/auctions/3.png';
// Importing images for the parking project
import plateNumber from '@/assets/images/ParkingSystem/plateNumber.jpg';
import dashboard from '@/assets/images/ParkingSystem/dashboard.png';
import invoice from '@/assets/images/ParkingSystem/invoice.png';
import userInterface from '@/assets/images/ParkingSystem/user interface.png';

// Import images for the vegetable system project
import allProducts from '@/assets/images/vegetableShop/allProducts.jpg';
import customInvoice from '@/assets/images/vegetableShop/customInvoice.jpg';
import invoice1 from '@/assets/images/vegetableShop/image.png';
import orderPage from '@/assets/images/vegetableShop/orderPage.jpg';

const activeProject = ref(null)
const projects = [
  {
    title: 'Real Time Auctions Plateform',
    tagline: 'academic project',
    tech: '[Node.js , Express.js , Bootstrap , Socket.io , Vue.js]',
    desc: 'A real-time online auction platform that allows users to browse products, participate in live auctions, and place bids in real time. The system includes secure authentication, auction scheduling, real-time bidding and an admin dashboard for managing auctions and products.',
    image: auction2,
    images: [
      auction2,
      auction1,
      auction3,
      auction4
    ],
    href: '#'
  },
  {
    title: 'Parking System',
    tagline: 'Thesis project',
    tech: '[Python , OpenCV , YOLOv8 , MySQL , EasyOCR , Node.js , HTML , CSS , JavaScript]',
    desc: 'An AI-powered parking management system that automates vehicle entry and exit using license plate recognition. The system manages parking spaces, calculates parking fees, tracks vehicle records, and provides an admin dashboard for monitoring parking operations.',
    image: plateNumber,
    images: [
      plateNumber,
      dashboard,
      invoice,
      userInterface
    ],
    href: '#'
  },
  {
    title: 'Vegetable Shop Management System',
    tagline: 'Personal project',
    tech: '[PHP , Laravel , MySQL , Bootstrap , HTML , CSS , JavaScript]',
    desc: 'A web-based management system designed to help vegetable shops efficiently manage products, inventory, sales, customers, and daily business operations. The system allows shop staff to manage vegetable stock, track product prices, process orders, and monitor sales records through an easy-to-use dashboard.',
    image: orderPage,
    images: [
      orderPage,
      allProducts,
      customInvoice,
      invoice1
    ],
    href: '#'
  }
]

const openProject = (project) => {
  activeProject.value = project;
  console.log('Opening project:', project.title);
}
</script>

<template>
  <section id="projects" class="projects">
    <div class="container">
      <div class="projects-heading">
        <div>
          <p class="eyebrow">Featured Projects</p>
          <!-- <h2 class="section-heading">Selected Work</h2> -->
        </div>
        <!-- <a href="#" class="view-all d-none">View all projects <i class="fa-solid fa-arrow-right"></i></a> -->
      </div>

      <div class="projects-grid">
        <article v-for="p in projects" :key="p.title" class="project-card">
          <div class="project-thumb">
            <img :src="p.image" :alt="p.tagline" />
            <button @click="openProject(p)" class="expand-btn" aria-label="Open project"><i class="fa-solid fa-up-right-from-square"></i></button>
          </div>
          <h3>{{ p.title }}</h3>
          <p>{{ p.tech}}</p>
          <!-- <p>{{ p.desc }}</p> -->
        </article>
      </div>
      <div>
        <ProjectModal :project="activeProject" @close="activeProject = null" />
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects {
  padding: 72px 0;
  border-top: 1px solid var(--border);
}

.projects-heading {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 36px;
}

.view-all {
  color: var(--accent);
  font-weight: 600;
  font-size: 0.9rem;
  white-space: nowrap;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

.project-thumb {
  position: relative;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 16px;
  aspect-ratio: 4 / 3;
}

.project-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.expand-btn {
  position: absolute;
  right: 12px;
  bottom: 12px;
  width: 34px;
  height: 34px;
  display: grid;
  place-items: center;
  background: rgba(10, 10, 10, 0.75);
  color: var(--text-0);
  border-radius: 8px;
  font-size: 0.85rem;
}

.project-card h3 {
  font-family: var(--font-display);
  font-size: 1.05rem;
  margin-bottom: 6px;
}

.project-card p {
  color: var(--text-1);
  font-size: 0.88rem;
}

@media (max-width: 900px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }

  .projects-heading {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }
}
</style>
