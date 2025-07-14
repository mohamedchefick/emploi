<template>
  <div>

    <!-- Section Bienfaits -->
    <section class="py-16 bg-[#da9a90da]">
      <div class="container mx-auto px-4 text-center max-w-7xl">

        <h3 class="text-3xl md:text-4xl font-bold mb-12">
          Pourquoi choisir <span class="text-black">Azɔ̌ ce</span> ?
        </h3>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div
            v-for="(item, index) in avantages"
            :key="'avantage-' + index"
            class="p-8 bg-white bg-opacity-10 rounded-3xl shadow-lg hover:shadow-2xl border-2 border-transparent hover:border-gray-500 transition-all duration-300"
          >
            <h4 class="text-xl md:text-2xl font-semibold mb-4">{{ item.title }}</h4>
            <p class="text-sm md:text-base leading-relaxed">{{ item.description }}</p>
          </div>
        </div>

      </div>
    </section>

<section
  class="relative py-24 bg-cover bg-center bg-no-repeat"
  :style="{ backgroundImage: `url(${IMGAvenir})` }"
>
  <div class="absolute inset-0 bg-opacity-60"></div>

  <div class="relative container mx-auto px-4 text-center max-w-6xl ">
    <h3 class="text-3xl md:text-4xl font-bold mb-12">
      L'avenir de <span class="text-[#da9a90] bg-black rounded-xl p-2">Azɔ̌ ce</span>
    </h3>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <div
        v-for="(item, index) in futur"
        :key="'avenir-bg-' + index"
        class="p-8 bg-white bg-opacity-10 backdrop-blur-lg rounded-3xl shadow-lg hover:bg-opacity-20 transition duration-300 transform hover:scale-105"
      >
        <h4 class="text-xl md:text-2xl font-semibold mb-4">{{ item.title }}</h4>
        <p class="text-sm md:text-base leading-relaxed">{{ item.description }}</p>
      </div>
    </div>

    <div class="mt-12">
      <p class="text-base md:text-lg bg-white rounded-xl py-2">
        <span class="font-semibold text-[#da9a90]">Notre vision :</span>
        devenir la plateforme de référence pour l'emploi et le développement professionnel en Afrique de l'Ouest.
      </p>
    </div>
  </div>
</section>


  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import IMGAvenir from "../assets/image/bgAvenir.webp"

const avantages = ref([
  {
    title: 'Accès Simplifié',
    description: 'Une interface claire et intuitive pour une expérience utilisateur optimale, accessible à tous les profils.',
  },
  {
    title: 'Offres Actualisées',
    description: 'Des offres d\'emploi récentes, vérifiées, et adaptées au marché béninois pour booster votre carrière.',
  },
  {
    title: '100% Gratuit',
    description: 'Profitez d\'un service entièrement gratuit, sans abonnement ni frais cachés.',
  },
  {
    title: 'Communauté Active',
    description: 'Rejoignez une communauté grandissante de chercheurs d\'emploi et d\'entreprises locales.',
  },
  {
    title: 'Support Personnalisé',
    description: 'Une équipe dédiée pour vous accompagner et répondre à vos besoins spécifiques.',
  },
  {
    title: 'Confidentialité Garantie',
    description: 'Vos données personnelles sont sécurisées et protégées selon les normes les plus strictes.',
  },
])

const futur = ref([
  {
    title: 'Générateur de CV Intelligent',
    description: 'Créez un CV professionnel en quelques clics grâce à notre générateur automatisé, adapté au marché local et international.',
  },
  {
    title: 'Résultats par Email',
    description: 'Recevez directement les résultats de vos candidatures par mail pour ne manquer aucune opportunité.',
  },
  {
    title: 'Alerte Nouvelles Offres',
    description: 'Soyez notifié en temps réel dès qu\'une nouvelle offre correspondant à votre profil est disponible.',
  },
])
</script>
