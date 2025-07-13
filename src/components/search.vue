<template>
  <section
    class="py-20 xl:py-40 text-center text-[#FFFFFF] bg-cover bg-center relative"
    :style="`background-image: url('${IMGCotonou}');`"
  >
    <div class="absolute inset-0 bg-[#da9a90]/70"></div>
    <div class="relative z-10">
      <h2 class="text-5xl font-extrabold mb-4">Trouvez l'emploi qui vous ressemble au Bénin</h2>
      <p class="text-lg max-w-2xl mx-auto mb-8 px-2">Azɔ̌ ce vous connecte avec des milliers d'opportunités dans tous les secteurs d'activité. Simple, rapide et gratuit.</p>
      <button @click="showModal = true" class="bg-[#FFFFFF] text-[#da9a90] px-6 py-3 rounded-full font-semibold hover:bg-[#f1e5e3] transition cursor-pointer">
        Cliquez pour la recherche
      </button>
    </div>
  </section>

  <!-- Modal avec arrière-plan transparent -->
  <div v-if="showModal" class="fixed inset-0 bg-gray bg-opacity-40 backdrop-blur-sm flex items-center justify-center z-50 px-4">
    <div class="bg-white/95 backdrop-blur-md text-[#000000] rounded-2xl p-8 max-w-2xl w-full shadow-2xl relative border border-white/20">

      <button @click="showModal = false" class="absolute top-4 right-4 text-[#da9a90] text-2xl font-bold hover:text-black transition-colors cursor-pointer">&times;</button>

      <h3 class="text-2xl font-bold mb-6 text-[#da9a90]">Affinez votre recherche</h3>

      <div class="space-y-5 text-left">
        <div>
          <label class="block font-semibold mb-2">Titre du poste</label>
          <input 
            v-model="jobTitle"
            type="text" 
            placeholder="Ex: Développeur, Secrétaire..."
            class="w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] placeholder:text-gray-400 bg-white/80"
          />
        </div>

        <!-- Multiselect pour les villes -->
        <div>
          <label class="block font-semibold mb-2">Villes</label>
          <div class="relative">
            <div @click="toggleCitiesDropdown" class="w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] bg-white/80 cursor-pointer flex justify-between items-center">
              <span v-if="selectedCities.length === 0" class="text-gray-400">Sélectionnez une ou plusieurs villes</span>
              <span v-else class="text-gray-700">{{ selectedCities.length }} ville(s) sélectionnée(s)</span>
              <svg class="w-5 h-5 text-gray-400 transition-transform" :class="{ 'rotate-180': showCitiesDropdown }" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
              </svg>
            </div>
            
            <!-- Liste déroulante des villes -->
            <div v-if="showCitiesDropdown" class="absolute z-10 w-full mt-1 bg-white/95 backdrop-blur-md border border-[#da9a90] rounded-lg shadow-lg max-h-60 overflow-y-auto">
              <div v-for="city in cities" :key="city" @click="toggleCity(city)" 
                   class="p-3 hover:bg-[#da9a90]/10 cursor-pointer flex items-center justify-between transition-colors">
                <span>{{ city }}</span>
                <svg v-if="selectedCities.includes(city)" class="w-5 h-5 text-[#da9a90]" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
                </svg>
              </div>
            </div>
          </div>
          
          <!-- Tags des villes sélectionnées -->
          <div v-if="selectedCities.length > 0" class="mt-2 flex flex-wrap gap-2">
            <span v-for="city in selectedCities" :key="city" 
                  class="inline-flex items-center px-3 py-1 rounded-full text-sm bg-[#da9a90] text-white">
              {{ city }}
              <button @click="removeCity(city)" class="ml-2 text-white hover:text-gray-200 transition-colors">×</button>
            </span>
          </div>
        </div>

        <!-- Multiselect pour les métiers -->
        <div>
        <label class="block font-semibold mb-2">Métiers</label>
            <div class="relative">
                <input
                v-model="jobInput"
                @focus="showJobsDropdown = true"
                @input="showJobsDropdown = true"
                type="text"
                placeholder="Tapez pour rechercher un métier"
                class="w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] bg-white/80"
                />
                <!-- Liste déroulante des métiers filtrés -->
                <div v-if="showJobsDropdown && filteredJobs.length > 0" class="absolute z-10 w-full mt-1 bg-white/95 backdrop-blur-md border border-[#da9a90] rounded-lg shadow-lg max-h-60 overflow-y-auto">
                <div v-for="job in filteredJobs" :key="job" @click="addJob(job)"
                    class="p-3 hover:bg-[#da9a90]/10 cursor-pointer flex items-center justify-between transition-colors">
                    <span>{{ job }}</span>
                    <svg v-if="selectedJobs.includes(job)" class="w-5 h-5 text-[#da9a90]" fill="currentColor" viewBox="0 0 20 20">
                    <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
                    </svg>
                </div>
                </div>
            </div>
            <!-- Tags des métiers sélectionnés -->
            <div v-if="selectedJobs.length > 0" class="mt-2 flex flex-wrap gap-2">
                <span v-for="job in selectedJobs" :key="job"
                    class="inline-flex items-center px-3 py-1 rounded-full text-sm bg-[#da9a90] text-white">
                {{ job }}
                <button @click="removeJob(job)" class="ml-2 text-white hover:text-gray-200 transition-colors">×</button>
                </span>
            </div>
        </div>

        <!-- Select amélioré pour le type de contrat -->
        <div>
          <label class="block font-semibold mb-2">Type de contrat</label>
          <div class="relative">
            <select v-model="selectedContract" class="w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] bg-white/80 appearance-none cursor-pointer">
              <option value="">Sélectionnez un type de contrat</option>
              <option value="Temps plein">Temps plein</option>
              <option value="Temps partiel">Temps partiel</option>
              <option value="Stage">Stage</option>
              <option value="Freelance">Freelance</option>
            </select>
            <svg class="absolute right-3 top-1/2 transform -translate-y-1/2 w-5 h-5 text-gray-400 pointer-events-none" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
          </div>
        </div>
      </div>

      <button @click="applyFilters" class="mt-6 w-full bg-[#da9a90] text-white py-3 rounded-full font-semibold hover:bg-[#bb857d] transition-colors cursor-pointer">
        Rechercher
      </button>

    </div>
  </div>

  <!-- Résultats -->
  <SearchResults v-if="showResults" :filters="filters" />
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import SearchResults from './SearchResults.vue'
import IMGCotonou from '../assets/image/cotnou.webp'

// Données
const cities = [
  'Cotonou', 'Porto-Novo', 'Parakou', 'Abomey', 'Bohicon', 'Djougou', 'Natitingou', 'Kandi', 'Lokossa', 'Ouidah',
  'Abomey-Calavi', 'Malanville', 'Savalou', 'Banikoara', 'Bassila', 'Nikki', 'Tchaourou', 'Kétou', 'Pobè',
  'Comè', 'Allada', 'Bembèrèkè', 'Dogbo', 'Kouandé', 'Covè', 'Zagnanado', 'Aplahoué', 'Dassa-Zoumè', 'Tanguiéta'
]


// Jobs
const jobs = [
  'Développeur', 'Secrétaire', 'Infirmier', 'Enseignant', 'Commercial', 'Électricien', 'Comptable', 'Chauffeur',
  'Cuisinier', 'Community Manager', 'Informatique'
]

// États
const showModal = ref(false)
const showResults = ref(false)
const showCitiesDropdown = ref(false)
const showJobsDropdown = ref(false)

const jobTitle = ref('')
const selectedCities = ref<string[]>([])
const selectedJobs = ref<string[]>([])
const selectedContract = ref('')

// Stockage des filtres
const filters = ref({
  jobTitle: '',
  cities: [] as string[],
  jobs: [] as string[],
  contract: ''
})

const jobInput = ref('')

const filteredJobs = computed(() =>
  jobs.filter(
    job =>
      job.toLowerCase().includes(jobInput.value.toLowerCase()) &&
      !selectedJobs.value.includes(job)
  )
)

const addJob = (job: string) => {
  if (!selectedJobs.value.includes(job)) {
    selectedJobs.value.push(job)
  }
  jobInput.value = ''
  showJobsDropdown.value = false
}

// Méthodes pour les villes
const toggleCitiesDropdown = () => {
  showCitiesDropdown.value = !showCitiesDropdown.value
  showJobsDropdown.value = false
}

const toggleCity = (city: string) => {
  const index = selectedCities.value.indexOf(city)
  if (index > -1) {
    selectedCities.value.splice(index, 1)
  } else {
    selectedCities.value.push(city)
  }
}

const removeCity = (city: string) => {
  const index = selectedCities.value.indexOf(city)
  if (index > -1) {
    selectedCities.value.splice(index, 1)
  }
}

// Méthodes pour les métiers
// const toggleJobsDropdown = () => {
//   showJobsDropdown.value = !showJobsDropdown.value
//   showCitiesDropdown.value = false
// }

// const toggleJob = (job: string) => {
//   const index = selectedJobs.value.indexOf(job)
//   if (index > -1) {
//     selectedJobs.value.splice(index, 1)
//   } else {
//     selectedJobs.value.push(job)
//   }
// }

const removeJob = (job: string) => {
  const index = selectedJobs.value.indexOf(job)
  if (index > -1) {
    selectedJobs.value.splice(index, 1)
  }
}

// Fermer les dropdowns en cliquant à l'extérieur
const handleClickOutside = (event: Event) => {
  const target = event.target as HTMLElement
  if (!target.closest('.relative')) {
    showCitiesDropdown.value = false
    showJobsDropdown.value = false
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})

// Méthodes principales
const applyFilters = () => {
  filters.value = {
    jobTitle: jobTitle.value,
    cities: selectedCities.value,
    jobs: selectedJobs.value,
    contract: selectedContract.value
  }
  showModal.value = false
  showResults.value = true
}
</script>