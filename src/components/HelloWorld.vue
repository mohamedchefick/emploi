<script setup lang="ts">
import Header from './header.vue';
import search from './search.vue';
import Bienfaits from './Bienfaits.vue';
import Domaine from "./Domaine.vue";
</script>

<template>
  <div class="min-h-screen bg-[#FFFFFF] text-[#000000] font-sans scroll-smooth">

    <!-- Header -->
    <Header />

    <!-- Accueil Section -->
    <section id="home" class="">
      <search />
    </section>

    <!-- Domaines Section -->
    <section id="domaines" class="">
      <Domaine />
    </section>

    <!-- Bienfaits Section -->
    <section id="bienfaits" class="">
      <Bienfaits />
    </section>

    <!-- Contact Section -->
    <section id="contact" class="py-16 bg-[#f5f5f5]">
      <div class="container mx-auto px-4 text-center max-w-3xl">
        <h2 class="text-3xl md:text-4xl font-bold mb-6">Contact</h2>
        <p class="text-base md:text-lg mb-4">
          Vous pouvez nous contacter pour toute question ou suggestion concernant <span class="text-[#da9a90] font-semibold">Azɔ̌ ce</span>.
        </p>
        <p class="text-sm text-gray-400 cursor-not-allowed inline-block">contact@azoce.com</p>
      </div>
    </section>

    <!-- Footer -->
    <footer id="profile" class="bg-[#000000] text-[#FFFFFF] py-6">
      <div class="container mx-auto px-4 text-center">
        <p class="text-sm">&copy; 2025 Azɔ̌ ce. Tous droits réservés.</p>
      </div>
    </footer>

  </div>
</template>

<style scoped>
html {
  scroll-behavior: smooth;
}
</style>
