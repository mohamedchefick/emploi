<template>
  <header class="] py-6 shadow-md">
    <div class="container mx-auto px-4 flex justify-between items-center">
      <h1 class="text-3xl font-bold text-[#000]">Azɔ̌ ce</h1>

      <!-- Menu Desktop -->
      <nav class="hidden md:block">
        <ul class="flex space-x-6 text- font-semibold">
          <li>
            <a href="#home" class="relative inline-block after:absolute after:left-0 after:bottom-0 after:h-[2px] after:w-0 after:bg-[#c88a80] after:transition-all after:duration-300 after:ease-in-out hover:after:w-full hover:-translate-y-1 hover:text-[#c88a80] transition-transform duration-300">Accueil</a>
          </li>
          <li>
            <a href="#domaines" class="relative inline-block after:absolute after:left-0 after:bottom-0 after:h-[2px] after:w-0 after:bg-[#c88a80] after:transition-all after:duration-300 after:ease-in-out hover:after:w-full hover:-translate-y-1 hover:text-[#c88a80] transition-transform duration-300">Domaines</a>
          </li>
          <li>
            <a href="#bienfaits" class="relative inline-block after:absolute after:left-0 after:bottom-0 after:h-[2px] after:w-0 after:bg-[#c88a80] after:transition-all after:duration-300 after:ease-in-out hover:after:w-full hover:-translate-y-1 hover:text-[#c88a80] transition-transform duration-300">Pourquoi Azɔ̌ ce</a>
          </li>
          <li>
            <a href="#contact" class="relative inline-block after:absolute after:left-0 after:bottom-0 after:h-[2px] after:w-0 after:bg-[#c88a80] after:transition-all after:duration-300 after:ease-in-out hover:after:w-full hover:-translate-y-1 hover:text-[#c88a80] transition-transform duration-300">Contact</a>
          </li>
          <li>
            <a href="#profile" class="flex items-center hover:-translate-y-1 transition-transform duration-300">
              <UserButton />
            </a>
          </li>
        </ul>
      </nav>

      <!-- Bouton Burger Mobile -->
      <button @click="toggleMenu" class="md:hidden text- focus:outline-none">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none"
          viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
            d="M4 6h16M4 12h16M4 18h16" />
        </svg>
      </button>
    </div>

    <!-- Menu Mobile -->
    <div v-if="menuOpen" class="md:hidden bg-[#] px-4 pb-4">
      <ul class="space-y-4 text-[#000] font-semibold text-center">
        <li><a href="#home" @click="toggleMenu" class="hover:underline">Accueil</a></li>
        <li><a href="#domaines" @click="toggleMenu" class="hover:underline">Domaines</a></li>
        <li><a href="#bienfaits" @click="toggleMenu" class="hover:underline">Pourquoi Azɔ̌ ce</a></li>
        <li><a href="#contact" @click="toggleMenu" class="hover:underline">Contact</a></li>
        <li><a href="#profile" @click="toggleMenu"><UserButton /></a></li>
      </ul>
    </div>
  </header>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { UserButton } from '@clerk/vue'

const menuOpen = ref(false)

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value
}
</script>