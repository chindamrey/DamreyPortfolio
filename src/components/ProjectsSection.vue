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

// Import images for the Merge PDF project
import mergeScreen from '@/assets/images/mergePDF/mergeScreen.png';
import addFile from '@/assets/images/mergePDF/addFile.png';
import PreviewScreen from '@/assets/images/mergePDF/PreviewScreen.png';


const activeProject = ref(null)
const projects = [
  {
    title: 'Real Time Auctions Plateform',
    tagline: 'academic project',
    tech: '[Node.js , Express.js , Bootstrap , Socket.io , Vue.js]',
    desc: 'A full-stack real-time online auction platform that allows users to browse products, participate in live auctions, place bids, and track auction activity in real time. The platform manages the complete auction lifecycle, including upcoming, active, and ended auctions, while providing secure authentication, bidding history, product management, and real-time communication between users and the server.',
    features: [
      {
        title: '🔐 User Authentication',
        description:
          'Implemented JWT-based authentication to securely identify users and protect authenticated resources.'
      },

      {
        title: '📦 Product Management',
        description:
          'Allows products to be created and managed with product information, images, auction details, starting prices, and auction schedules.'
      },

      {
        title: '🔨 Real-Time Bidding',
        description:
          'Implemented Socket.IO to broadcast new bids instantly to users participating in the same auction without requiring page refreshes.'
      },

      {
        title: '⏱️ Auction Lifecycle',
        description:
          'Manages auctions through upcoming, live, and ended states based on configured start and end times.'
      },

      {
        title: '📜 Bid History',
        description:
          'Records bidding activity and allows users to track previous bids and auction activity.'
      },

      {
        title: '📡 Auction Rooms',
        description:
          'Uses Socket.IO rooms to separate real-time communication for individual auctions and prevent unrelated users from receiving unnecessary events.'
      },

      {
        title: '🏆 Winner and Auction Result',
        description:
          'Determines the winning bid when an auction ends and maintains the final auction result for future reference.'
      },

      {
        title: '👤 User Auction Tracking',
        description:
          'Allows users to monitor auctions they are participating in and track relevant auction activity.'
      }
    ],
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
    title: 'Parking Management System',
    tagline: 'Thesis project',
    tech: '[Python , OpenCV , YOLOv8 , MySQL , EasyOCR , Node.js ,Vue.js, JS, Rest API]',
    desc: 'An AI-powered web-based parking management system that automates vehicle entry and exit using license plate recognition. The system manages parking spaces, calculates parking fees, tracks vehicle records, processes Bakong KHQR payments, and provides an admin dashboard for monitoring parking operations.',
    features: [
      {
        title: '🚗 Automatic License Plate Recognition',
        description:
          'Uses YOLOv8 and OCR technology to detect and recognize vehicle license plates for automated vehicle identification.'
      },
      {
        title: '🅿️ Parking Space Management',
        description:
          'Tracks available and occupied parking spaces and manages vehicle parking sessions.'
      },
      {
        title: '💰 Automatic Parking Fee Calculation',
        description:
          'Automatically calculates parking fees based on vehicle parking duration.'
      },
      {
        title: '📱 Bakong KHQR Payment',
        description:
          'Integrates Bakong KHQR to process parking payments and verify payment status before vehicle exit.'
      },
      {
        title: '📊 Admin Dashboard',
        description:
          'Provides administrators with parking occupancy, vehicle records, parking sessions, and payment information.'
      }
    ],
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
    title: 'Merge PDF',
    tagline: 'Personal project',
    tech: '[Vue.js, PDF-lib, Bootstrap , HTML , CSS , JavaScript]',
    desc: 'A web-based management system designed to help vegetable shops efficiently manage products, inventory, sales, customers, and daily business operations. The system allows shop staff to manage vegetable stock, track product prices, process orders, and monitor sales records through an easy-to-use dashboard.',
    features: [
      {
        title: '📄 PDF File Selection',
        description:
          'Allows users to select and add multiple PDF files for merging.'
      },
      {
        title: '🔀 Merge Multiple PDFs',
        description:
          'Combines multiple PDF documents into a single PDF file while preserving the pages from each selected document.'
      },
      {
        title: '👀 PDF Preview',
        description:
          'Provides a PDF preview so users can review the selected document before completing the merge process.'
      },
      {
        title: '🗂️ File Management',
        description:
          'Allows users to review selected files, remove unwanted files, and manage the documents before merging.'
      },
      {
        title: '⚡ Client-Side Processing',
        description:
          'Processes PDF files directly in the browser without requiring a backend server or database.'
      },
      {
        title: '🔒 Privacy-Focused',
        description:
          'PDF files do not need to be uploaded to a server, helping keep users’ documents on their own device.'
      },
      {
        title: '✏️ Custom Output Filename',
        description:
          'Allows users to specify the filename of the generated merged PDF document.'
      },
      {
        title: '⬇️ PDF Download',
        description:
          'Generates the merged PDF and allows users to download the final document directly to their device.'
      }
    ],
    image: mergeScreen,
    images: [
      mergeScreen,
      addFile,
      PreviewScreen
    ],
    href: 'https://pdf-toolkit-dusky-two.vercel.app/'
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
          <p class="eyebrow">Recent Projects</p>
          <!-- <h2 class="section-heading">Selected Work</h2> -->
        </div>
        <!-- <a href="#" class="view-all d-none">View all projects <i class="fa-solid fa-arrow-right"></i></a> -->
      </div>

      <div class="projects-grid">
        <article v-for="p in projects" :key="p.title" class="project-card">
          <div class="project-thumb">
            <img :src="p.image" :alt="p.tagline" />
            <button @click="openProject(p)" class="expand-btn" aria-label="Open project"><i
                class="fa-solid fa-up-right-from-square"></i></button>
          </div>
          <a :href="p.href" target="_blank" rel="noopener noreferrer" class="tooltip">
            <span class="tooltip-text">Visit Now!</span>
            <h3>{{ p.title }}</h3>
          </a>
          <p>{{ p.tech }}</p>
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

.expand-btn:hover {
  color: var(--accent);
}

.tooltip {
  position: relative;
}

.tooltip-text {
  position: absolute;
  bottom: 30px;
  left: 20%;
  transform: translateX(-50%);

  background: #222;
  color: white;
  padding: 5px 10px;
  border-radius: 5px;

  opacity: 0;
  visibility: hidden;
  transition: 0.2s;
  white-space: nowrap;
}

.tooltip:hover .tooltip-text {
  opacity: 1;
  visibility: visible;
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
