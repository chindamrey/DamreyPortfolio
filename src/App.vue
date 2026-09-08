<script setup>
import TheHeader from './components/TheHeader.vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import ServicesSection from './components/ServicesSection.vue'
import TechStackSection from './components/TechStackSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import StatsSection from './components/StatsSection.vue'
import TestimonialsContactSection from './components/TestimonialsContactSection.vue'
import EducationSection from './components/EducationSection.vue'
import TheFooter from './components/TheFooter.vue'
</script>

<template>
  <TheHeader />
  <main>
    <HeroSection />
    <AboutSection />
    <ServicesSection />
    <TechStackSection />
    <ProjectsSection />
    <StatsSection />
    <EducationSection />
    <!-- <TestimonialsContactSection /> -->
  </main>
  <TheFooter />
</template>
