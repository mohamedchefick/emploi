<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import { SignedIn, SignedOut, SignInButton } from '@clerk/vue'

</script>

<template>
  
  <SignedOut>
    <div class="min-h-screen bg-[#FFFFFF] flex items-center justify-center italic">
      <div class="text-center">
        <h1 class="text-3xl font-bold text-[#000000] mb-4">Bienvenue sur Azɔ̌ ce</h1>
        <p class="text-lg text-gray-600 mb-8">Connectez-vous pour accéder aux offres d'emploi</p>
        <SignInButton mode="modal">
          <button class="bg-[#da9a90] text-white px-8 py-3 rounded-lg hover:bg-[#c88a80] hover:-translate-y-1 hover:-translate-x-1 transition duration-300 cursor-pointer">
            Se connecter
          </button>
        </SignInButton>
      </div>
    </div>
  </SignedOut>
  
  <SignedIn>
    <div class="italic">
      <HelloWorld />
    </div>
  </SignedIn>
</template>

<style scoped>

</style>
