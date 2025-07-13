<template>
  <div>
    <section class="py-16 bg-[#FFFFFF]">
      <div class="container mx-auto px-4">
        <h3 class="text-3xl font-bold text-center mb-12 text-[#000000]">Les Domaines Populaires</h3>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div
            v-for="(domaine, index) in domaines"
            :key="index"
            class="p-6 rounded-lg shadow-lg hover:shadow-2xl transition"
          >
            <h4 class="text-xl font-semibold mb-4 text-[#da9a90]">{{ domaine.titre }}</h4>
            <p class="text-sm">{{ domaine.description }}</p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
const domaines = [
  {
    titre: "Informatique & Technologie",
    description: "Développement Web, IA, Réseaux, Sécurité informatique et plus encore.",
  },
  {
    titre: "Finance & Comptabilité",
    description: "Banque, Assurance, Analyse financière, Audit, Comptabilité générale.",
  },
  {
    titre: "Éducation & Formation",
    description: "Enseignement, Formation professionnelle, Coaching, Conseil pédagogique.",
  },
  {
    titre: "Santé & Bien-être",
    description: "Médecine, Paramédical, Bien-être, Psychologie, Nutrition.",
  },
  {
    titre: "BTP & Génie Civil",
    description: "Construction, Architecture, Urbanisme, Travaux publics.",
  },
  {
    titre: "Commerce & Vente",
    description: "Marketing, Distribution, Vente en ligne, Négociation commerciale.",
  },
];
</script>