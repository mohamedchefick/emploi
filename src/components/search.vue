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
        <!-- Mots-clés -->
        <div>
          <label for="keywords" class="block font-semibold mb-2">Mots-clés</label>
          <input 
            id="keywords"
            v-model="keywords"
            name="keywords"
            type="text" 
            placeholder="Mots-clés"
            class="search-input form-text w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] placeholder:text-gray-400 bg-white/80"
            autocomplete="off"
            maxlength="128"
          />
        </div>

        <!-- Région -->
        <div>
          <label for="region" class="block font-semibold mb-2">Région(s)</label>
          <div class="relative">
            <select 
              id="region" 
              name="region_filter" 
              v-model="selectedRegion"
              class="form-select w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] bg-white/80 appearance-none cursor-pointer"
            >
              <option value="" selected>Région(s)</option>
              <option value="57">Abomey</option>
              <option value="58">Abomey-Calavi</option>
              <option value="59">Aplahoué</option>
              <option value="60">Cotonou</option>
              <option value="61">Djougou</option>
              <option value="62">Kandi</option>
              <option value="63">Lokossa</option>
              <option value="64">Natitingou</option>
              <option value="65">Parakou</option>
              <option value="943">Pobè</option>
              <option value="944">Porto-Novo</option>
              <option value="945">Savalou</option>
              <option value="947">International</option>
            </select>
            <svg class="absolute right-3 top-1/2 transform -translate-y-1/2 w-5 h-5 text-gray-400 pointer-events-none" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
          </div>
        </div>

        <!-- Métier -->
        <div>
          <label for="job" class="block font-semibold mb-2">Métier</label>
          <div class="relative">
            <select 
              id="job" 
              name="job_filter" 
              v-model="selectedJob"
              class="form-select w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] bg-white/80 appearance-none cursor-pointer"
            >
              <option value="" selected>Métier</option>
              <option value="1127">Achats</option>
              <option value="29">Commercial, vente</option>
              <option value="30">Gestion, comptabilité, finance</option>
              <option value="31">Informatique, nouvelles technologies</option>
              <option value="1115">Juridique</option>
              <option value="32">Management, direction générale</option>
              <option value="33">Marketing, communication</option>
              <option value="34">Métiers de la santé et du social</option>
              <option value="35">Métiers des services</option>
              <option value="36">Métiers du BTP</option>
              <option value="37">Production, maintenance, qualité</option>
              <option value="39">R&D, gestion de projets</option>
              <option value="38">RH, formation</option>
              <option value="40">Secrétariat, assistanat</option>
              <option value="525">Télémarketing, téléassistance</option>
              <option value="41">Tourisme, hôtellerie, restauration</option>
              <option value="28">Transport, logistique</option>
            </select>
            <svg class="absolute right-3 top-1/2 transform -translate-y-1/2 w-5 h-5 text-gray-400 pointer-events-none" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
          </div>
        </div>

        <!-- Type de contrat (optionnel) -->
        <div>
          <label for="contract" class="block font-semibold mb-2">Type de contrat</label>
          <div class="relative">
            <select 
              id="contract"
              v-model="selectedContract" 
              class="w-full border border-[#da9a90] rounded-lg p-3 focus:outline-none focus:ring-2 focus:ring-[#da9a90] bg-white/80 appearance-none cursor-pointer"
            >
              <option value="">Sélectionnez un type de contrat</option>
              <option value="Temps plein">Temps plein</option>
              <option value="Temps partiel">Temps partiel</option>
              <option value="Stage">Stage</option>
              <option value="Freelance">Freelance</option>
              <option value="CDD">CDD</option>
              <option value="CDI">CDI</option>
            </select>
            <svg class="absolute right-3 top-1/2 transform -translate-y-1/2 w-5 h-5 text-gray-400 pointer-events-none" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
            </svg>
          </div>
        </div>

        <!-- Résumé des filtres sélectionnés -->
        <div v-if="hasFilters" class="mt-4 p-4 bg-[#da9a90]/10 rounded-lg">
          <h4 class="font-semibold text-[#da9a90] mb-2">Filtres sélectionnés :</h4>
          <div class="flex flex-wrap gap-2">
            <span v-if="keywords" class="inline-flex items-center px-3 py-1 rounded-full text-sm bg-[#da9a90] text-white">
              Mots-clés: {{ keywords }}
            </span>
            <span v-if="selectedRegion" class="inline-flex items-center px-3 py-1 rounded-full text-sm bg-[#da9a90] text-white">
              Région: {{ getRegionName(selectedRegion) }}
            </span>
            <span v-if="selectedJob" class="inline-flex items-center px-3 py-1 rounded-full text-sm bg-[#da9a90] text-white">
              Métier: {{ getJobName(selectedJob) }}
            </span>
            <span v-if="selectedContract" class="inline-flex items-center px-3 py-1 rounded-full text-sm bg-[#da9a90] text-white">
              Contrat: {{ selectedContract }}
            </span>
          </div>
        </div>
      </div>

      <div class="flex gap-4 mt-6">
        <button @click="clearFilters" class="flex-1 bg-gray-200 text-gray-700 py-3 rounded-full font-semibold hover:bg-gray-300 transition-colors cursor-pointer">
          Réinitialiser
        </button>
        <button @click="applyFilters" class="flex-1 bg-[#da9a90] text-white py-3 rounded-full font-semibold hover:bg-[#bb857d] transition-colors cursor-pointer">
          Rechercher
        </button>
      </div>
    </div>
  </div>

  <!-- Résultats -->
  <SearchResults v-if="showResults" :filters="filters" />
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import SearchResults from './SearchResults.vue'
import IMGCotonou from '../assets/image/cotnou.webp'

// États
const showModal = ref(false)
const showResults = ref(false)

// Champs de recherche
const keywords = ref('')
const selectedRegion = ref('')
const selectedJob = ref('')
const selectedContract = ref('')

// Stockage des filtres
const filters = ref({
  keywords: '',
  region: '',
  job: '',
  contract: ''
})

// Mappings pour les noms d'affichage
const regionMap = {
  '57': 'Abomey',
  '58': 'Abomey-Calavi',
  '59': 'Aplahoué',
  '60': 'Cotonou',
  '61': 'Djougou',
  '62': 'Kandi',
  '63': 'Lokossa',
  '64': 'Natitingou',
  '65': 'Parakou',
  '943': 'Pobè',
  '944': 'Porto-Novo',
  '945': 'Savalou',
  '947': 'International'
}

const jobMap = {
  '1127': 'Achats',
  '29': 'Commercial, vente',
  '30': 'Gestion, comptabilité, finance',
  '31': 'Informatique, nouvelles technologies',
  '1115': 'Juridique',
  '32': 'Management, direction générale',
  '33': 'Marketing, communication',
  '34': 'Métiers de la santé et du social',
  '35': 'Métiers des services',
  '36': 'Métiers du BTP',
  '37': 'Production, maintenance, qualité',
  '39': 'R&D, gestion de projets',
  '38': 'RH, formation',
  '40': 'Secrétariat, assistanat',
  '525': 'Télémarketing, téléassistance',
  '41': 'Tourisme, hôtellerie, restauration',
  '28': 'Transport, logistique'
}

// Computed properties
const hasFilters = computed(() => {
  return keywords.value || selectedRegion.value || selectedJob.value || selectedContract.value
})

// Méthodes utilitaires
const getRegionName = (regionId: string) => {
  return regionMap[regionId as keyof typeof regionMap] || regionId
}

const getJobName = (jobId: string) => {
  return jobMap[jobId as keyof typeof jobMap] || jobId
}

// Méthodes principales
const applyFilters = () => {
  filters.value = {
    keywords: keywords.value,
    region: selectedRegion.value,
    job: selectedJob.value,
    contract: selectedContract.value
  }
  showModal.value = false
  showResults.value = true
}

const clearFilters = () => {
  keywords.value = ''
  selectedRegion.value = ''
  selectedJob.value = ''
  selectedContract.value = ''
}
</script>

<style scoped>
/* Styles pour améliorer l'apparence des selects */
.form-select {
  background-image: none;
}

.search-input {
  transition: all 0.3s ease;
}

.search-input:focus {
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(218, 154, 144, 0.2);
}

/* Animation pour le modal */
.modal-enter-active, .modal-leave-active {
  transition: all 0.3s ease;
}

.modal-enter-from, .modal-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
</style>