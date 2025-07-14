<script setup lang="ts">
import { ref, watch, onMounted, computed } from 'vue'

interface JobOffer {
  label: string
  entreprise: string
  niveau_etude: string
  niveau_experience: string
  type_contrat: string
  regions: string
  competance: string
  date: string
  lien_de_redirection: string
}

interface Filters {
  keywords?: string
  region?: string
  job?: string
  contract?: string
}

const props = defineProps<{ filters: Filters }>()

const results = ref<JobOffer[]>([])
const loading = ref(false)
const error = ref<string | null>(null)
const currentPage = ref(1)
const totalPages = ref(1)
const hasNextPage = ref(true)

const API_BASE_URL = 'https://tools.remaapp.com/api/v1/services/emploi-benin'

async function fetchResults(page: number = 1) {
  loading.value = true
  error.value = null

  try {
    const params = new URLSearchParams()
    
    // Ajouter les filtres à l'URL
    if (props.filters?.job) {
      params.append('metier', `'${props.filters.job}'`)
    }
    if (props.filters?.region) {
      params.append('region', `'${props.filters.region}'`)
    }
    
    params.append('page', page.toString())
    
    const response = await fetch(`${API_BASE_URL}?${params.toString()}`)
    
    if (!response.ok) {
      throw new Error(`Erreur HTTP: ${response.status}`)
    }
    
    const data: JobOffer[] = await response.json()
    
    if (page === 1) {
      results.value = data
    } else {
      results.value = [...results.value, ...data]
    }
    
    // Vérifier s'il y a une page suivante
    hasNextPage.value = data.length > 0
    
    // Si on a moins de résultats que prévu, c'est probablement la dernière page
    if (data.length < 10) { // Assumons 10 résultats par page
      hasNextPage.value = false
    }
    
    currentPage.value = page
    
  } catch (err) {
    error.value = err instanceof Error ? err.message : 'Erreur inconnue'
    results.value = []
    hasNextPage.value = false
  } finally {
    loading.value = false
  }
}

const loadMoreResults = () => {
  if (!loading.value && hasNextPage.value) {
    fetchResults(currentPage.value + 1)
  }
}

const resetAndFetch = () => {
  currentPage.value = 1
  hasNextPage.value = true
  results.value = []
  fetchResults(1)
}

// Fonction pour filtrer les résultats côté client si nécessaire
const filteredResults = computed(() => {
  let filtered = results.value

  // Filtrer par mots-clés si spécifiés
  if (props.filters?.keywords) {
    const keywords = props.filters.keywords.toLowerCase()
    filtered = filtered.filter(job => 
      job.label.toLowerCase().includes(keywords) ||
      job.entreprise.toLowerCase().includes(keywords) ||
      job.competance.toLowerCase().includes(keywords)
    )
  }

  // Filtrer par type de contrat si spécifié
  if (props.filters?.contract) {
    filtered = filtered.filter(job => 
      job.type_contrat.toLowerCase().includes(props.filters.contract!.toLowerCase())
    )
  }

  return filtered
})

const formatDate = (dateString: string) => {
  const date = new Date(dateString)
  return date.toLocaleDateString('fr-FR', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

const getExperienceIcon = (experience: string) => {
  if (experience.includes('débutant') || experience.includes('0')) {
    return '🌱'
  } else if (experience.includes('2') || experience.includes('3')) {
    return '💼'
  } else if (experience.includes('5') || experience.includes('senior')) {
    return '👑'
  }
  return '📊'
}

watch(() => props.filters, resetAndFetch, { immediate: true, deep: true })
onMounted(resetAndFetch)
</script>

<template>
  <div class="bg-white py-12 px-6 max-w-6xl mx-auto">
    <h3 class="text-4xl font-extrabold text-[#CC5500] mb-8 tracking-tight">🔍 Résultats de Recherche</h3>

    <!-- Indicateurs de filtres actifs -->
    <div v-if="props.filters && Object.values(props.filters).some(v => v)" class="mb-6 p-4 bg-gray-50 rounded-lg">
      <h4 class="text-sm font-semibold text-gray-700 mb-2">Filtres actifs:</h4>
      <div class="flex flex-wrap gap-2">
        <span v-if="props.filters.keywords" class="px-3 py-1 bg-[#CC5500] text-white text-sm rounded-full">
          Mots-clés: {{ props.filters.keywords }}
        </span>
        <span v-if="props.filters.region" class="px-3 py-1 bg-[#da9a90] text-white text-sm rounded-full">
          Région: {{ props.filters.region }}
        </span>
        <span v-if="props.filters.job" class="px-3 py-1 bg-[#c1a07a] text-white text-sm rounded-full">
          Métier: {{ props.filters.job }}
        </span>
        <span v-if="props.filters.contract" class="px-3 py-1 bg-[#004953] text-white text-sm rounded-full">
          Contrat: {{ props.filters.contract }}
        </span>
      </div>
    </div>

    <div v-if="loading && results.length === 0" class="text-center text-lg text-gray-600 animate-pulse py-12">
      <div class="inline-block animate-spin rounded-full h-8 w-8 border-b-2 border-[#CC5500] mb-4"></div>
      <p>Chargement des offres...</p>
    </div>

    <div v-else-if="error" class="text-center text-red-500 text-lg font-medium py-12">
      <div class="bg-red-50 border border-red-200 rounded-lg p-6 max-w-md mx-auto">
        <h4 class="font-bold mb-2">Erreur de chargement</h4>
        <p>{{ error }}</p>
        <button @click="resetAndFetch" class="mt-4 bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600 transition">
          Réessayer
        </button>
      </div>
    </div>

    <div v-else>
      <!-- Compteur de résultats -->
      <div class="mb-6 text-gray-600">
        <p class="text-sm">
          {{ filteredResults.length }} offre(s) trouvée(s)
          <span v-if="hasNextPage" class="text-[#CC5500]">• Page {{ currentPage }}</span>
        </p>
      </div>

      <div v-if="filteredResults.length === 0" class="text-center text-gray-500 italic text-lg py-12">
        <div class="bg-gray-50 rounded-lg p-8 max-w-md mx-auto">
          <div class="text-4xl mb-4">🔍</div>
          <p>Aucun résultat correspondant à votre recherche.</p>
          <p class="text-sm mt-2">Essayez de modifier vos critères de recherche.</p>
        </div>
      </div>

      <div v-else class="grid grid-cols-1 gap-6">
        <div 
          v-for="(job, idx) in filteredResults" 
          :key="idx" 
          class="rounded-2xl shadow-lg hover:shadow-xl transition duration-300 p-6 bg-gradient-to-br from-white to-[#f9f9f9] border border-gray-100"
        >
          <!-- En-tête du poste -->
          <div class="flex justify-between items-start mb-4">
            <div class="flex-1">
              <h4 class="text-2xl font-bold text-[#004953] mb-2 leading-tight">
                {{ job.label }}
              </h4>
              <p class="text-lg text-[#CC5500] font-semibold">
                {{ job.entreprise }}
              </p>
            </div>
            <div class="text-right text-sm text-gray-500">
              <p>{{ formatDate(job.date) }}</p>
            </div>
          </div>

          <!-- Informations principales -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
            <div class="flex items-center text-gray-600">
              <svg class="w-5 h-5 mr-2 text-[#CC5500]" fill="currentColor" viewBox="0 0 20 20">
                <path d="M10 2a6 6 0 016 6c0 4.5-6 10-6 10S4 12.5 4 8a6 6 0 016-6z" />
              </svg>
              <span class="font-medium">{{ job.regions }}</span>
            </div>

            <div class="flex items-center text-gray-600">
              <svg class="w-5 h-5 mr-2 text-[#CC5500]" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path d="M17 9V7a5 5 0 00-10 0v2"></path>
                <path d="M5 9h14l1 12H4L5 9z"></path>
              </svg>
              <span class="font-medium">{{ job.type_contrat }}</span>
            </div>

            <div class="flex items-center text-gray-600">
              <svg class="w-5 h-5 mr-2 text-[#CC5500]" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path d="M12 14l9-5-9-5-9 5 9 5z"></path>
                <path d="M12 14l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14z"></path>
              </svg>
              <span class="font-medium">{{ job.niveau_etude }}</span>
            </div>

            <div class="flex items-center text-gray-600">
              <span class="text-lg mr-2">{{ getExperienceIcon(job.niveau_experience) }}</span>
              <span class="font-medium">{{ job.niveau_experience }}</span>
            </div>
          </div>

          <!-- Compétences -->
          <div v-if="job.competance" class="mb-4">
            <h5 class="text-sm font-semibold text-gray-700 mb-2">Compétences requises:</h5>
            <div class="flex flex-wrap gap-2">
              <span 
                v-for="skill in job.competance.split(' - ')" 
                :key="skill"
                class="inline-block bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded-full"
              >
                {{ skill }}
              </span>
            </div>
          </div>

          <!-- Tags et action -->
          <div class="flex flex-wrap gap-2 items-center justify-between">
            <div class="flex flex-wrap gap-2">
              <span class="inline-block bg-[#CC5500] text-white text-sm px-3 py-1 rounded-full shadow">
                {{ job.type_contrat }}
              </span>
              <span class="inline-block bg-[#c1a07a] text-white text-sm px-3 py-1 rounded-full shadow">
                {{ job.regions }}
              </span>
            </div>
            
            <a
              :href="job.lien_de_redirection"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex items-center bg-[#da9a90] hover:bg-[#bb857d] text-white text-sm px-4 py-2 rounded-full shadow transition-colors duration-300"
            >
              <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path>
              </svg>
              Voir l'offre
            </a>
          </div>
        </div>
      </div>

      <!-- Bouton "Charger plus" -->
      <div v-if="hasNextPage" class="text-center mt-8">
        <button 
          @click="loadMoreResults"
          :disabled="loading"
          class="bg-[#CC5500] hover:bg-[#b8470a] text-white px-8 py-3 rounded-full font-semibold transition-colors duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          <span v-if="loading" class="flex items-center">
            <div class="animate-spin rounded-full h-4 w-4 border-b-2 border-white mr-2"></div>
            Chargement...
          </span>
          <span v-else>Charger plus d'offres</span>
        </button>
      </div>

      <!-- Indicateur de fin -->
      <div v-if="!hasNextPage && filteredResults.length > 0" class="text-center mt-8 text-gray-500 text-sm">
        <p>Toutes les offres ont été chargées</p>
      </div>
    </div>
  </div>
</template>