<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
import { supabase } from '../supabase'

const props = defineProps<{ filters: {
  jobTitle?: string;
  cities?: string[];
  jobs?: string[];
  contract?: string;
} }>()

const results = ref<any[]>([])
const loading = ref(false)
const error = ref<string | null>(null)

async function fetchResults() {
  loading.value = true
  error.value = null
  let query = supabase.from('offres_emploi').select('*')

  if (props.filters?.jobTitle) {
    query = query.ilike('titre_poste', `%${props.filters.jobTitle}%`)
  }
  if (props.filters?.cities?.length) {
    query = query.ilike('ville', `%${props.filters.cities[0]}%`)
  }
  if (props.filters?.contract) {
    query = query.ilike('type_contrat', `%${props.filters.contract}%`)
  }
  if (props.filters?.jobs?.length) {
    query = query.ilike('metier', `%${props.filters.jobs[0]}%`)
  }

  const { data, error: supaError } = await query

  if (supaError) {
    error.value = supaError.message
    results.value = []
  } else {
    results.value = (data || []).map(({ id, created_at, ...rest }: any) => rest)
  }

  loading.value = false
}

watch(() => props.filters, fetchResults, { immediate: true, deep: true })
onMounted(fetchResults)
</script>

<template>
  <div class="bg-white py-12 px-6 max-w-6xl mx-auto">
    <h3 class="text-4xl font-extrabold text-[#CC5500] mb-8 tracking-tight">🔍 Résultats de Recherche</h3>

    <div v-if="loading" class="text-center text-lg text-gray-600 animate-pulse">
      Chargement des offres...
    </div>

    <div v-else-if="error" class="text-center text-red-500 text-lg font-medium">
      {{ error }}
    </div>

    <div v-else>
      <div v-if="results.length === 0" class="text-center text-gray-500 italic text-lg">
        Aucun résultat correspondant à votre recherche.
      </div>

      <div v-else class="grid grid-cols-1 gap-6">
        <div 
          v-for="(row, idx) in results" 
          :key="idx" 
          class=" rounded-2xl shadow-2xl hover:shadow-lg transition duration-300 p-6 bg-gradient-to-br from-white to-[#f9f9f9]"
        >
          <h4 class="text-2xl font-bold text-[#004953] mb-2">
            {{ row.titre_poste || 'Poste non spécifié' }}
          </h4>

          <p class="text-gray-600 mb-1 flex items-center">
            <svg class="w-5 h-5 mr-2 text-[#CC5500]" fill="currentColor" viewBox="0 0 20 20">
              <path d="M10 2a6 6 0 016 6c0 4.5-6 10-6 10S4 12.5 4 8a6 6 0 016-6z" />
            </svg>
            {{ row.ville || 'Ville non précisée' }}
          </p>

          <p class="text-gray-600 mb-1 flex items-center">
            <svg class="w-5 h-5 mr-2 text-[#CC5500]" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
              <path d="M17 9V7a5 5 0 00-10 0v2"></path>
              <path d="M5 9h14l1 12H4L5 9z"></path>
            </svg>
            {{ row.type_contrat || 'Type de contrat non précisé' }}
          </p>

          <p class="text-gray-600 mb-4 flex items-center">
            <svg class="w-5 h-5 mr-2 text-[#CC5500]" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
              <path d="M12 11c0 1.104-.896 2-2 2s-2-.896-2-2 .896-2 2-2 2 .896 2 2zM4 17h16M4 21h16"></path>
            </svg>
            {{ row.metier || 'Métier non précisé' }}
          </p>

          <div class="mt-4 flex flex-wrap gap-2">
            <span class="inline-block bg-[#CC5500] text-white text-sm px-3 py-1 rounded-full shadow truncate">
              {{ row.type_contrat || 'Contrat inconnu' }}
            </span>
            <span class="inline-block bg-[#c1a07a] text-white text-sm px-3 py-1 rounded-full shadow truncate">
              {{ row.ville || 'Ville inconnue' }}
            </span>
            <span class="inline-block bg-[#da9a90] text-white text-sm px-3 py-1 rounded-full shadow truncate">
              {{ row.metier || 'Métier inconnu' }}
            </span>
                        <a
              v-if="row.lien"
              :href="row.lien"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-block bg-[#da9a90b2] hover:bg-[#da9a90] text-white text-sm px-3 py-1 rounded-full shadow mt-2 cursor-pointer duration-300 truncate"
            >
              {{ row.lien }}
            </a>
            <span
              v-else
              class="inline-block bg-[#da9a90b2] text-white text-sm px-3 py-1 rounded-full shadow mt-2"
            >
              Métier inconnu
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template> 