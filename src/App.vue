<script setup>
import { ref, onMounted, watch } from 'vue'
import SearchBar from './components/SearchBar.vue'
import CharacterCard from './components/CharacterCard.vue'
import CharacterModal from './components/CharacterModal.vue'

const searchQuery = ref(localStorage.getItem('rick_search') || '')
const page = ref(Number(localStorage.getItem('rick_page')) || 1)

const characters = ref([])
const info = ref({ pages: 0, next: null, prev: null })
const isLoading = ref(false)
const selectedCharacter = ref(null)

const MAX_PAGES = 12

const fetchData = async () => {
  isLoading.value = true
  characters.value = [] 
  
  try {
    const params = new URLSearchParams()
    if (searchQuery.value) params.append('name', searchQuery.value)
    params.append('page', page.value.toString())

    const url = `https://rickandmortyapi.com/api/character/?${params.toString()}`
    const response = await fetch(url)
    const data = await response.json()
    
    if (data.results) {
      characters.value = data.results
      info.value = data.info
    } else {
      characters.value = []
      info.value = { pages: 0, next: null, prev: null }
    }
  } catch (error) {
    console.error("Portal Error:", error)
  } finally {
    isLoading.value = false
    window.scrollTo({ top: 0, behavior: 'smooth' })
  }
}

const updateSearch = (value) => {
  searchQuery.value = value
  page.value = 1
}

watch([searchQuery, page], () => {
  localStorage.setItem('rick_search', searchQuery.value)
  localStorage.setItem('rick_page', page.value.toString())
  fetchData()
})

onMounted(() => {
  fetchData()
})
</script>

<template>
  <div class="min-h-screen bg-slate-950 text-slate-200 pb-32">
    <header class="py-12 px-6 flex flex-col items-center text-center">
      <h1 class="text-5xl md:text-6xl font-black mb-8 bg-gradient-to-r from-green-400 to-emerald-600 bg-clip-text text-transparent italic tracking-tighter">
        RICK AND MORTY
      </h1>
      <SearchBar @search="updateSearch" />
    </header>

    <main class="max-w-7xl mx-auto px-6">
      <div v-if="isLoading" class="flex flex-col items-center justify-center py-20">
        <div class="animate-spin rounded-full h-16 w-16 border-t-4 border-green-500 mb-4 shadow-[0_0_15px_rgba(34,197,94,0.5)]"></div>
        <p class="text-green-500 font-mono animate-pulse font-bold tracking-widest">SYNCING DIMENSIONS...</p>
      </div>

      <div v-else-if="characters.length > 0" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
        <CharacterCard 
          v-for="char in characters" 
          :key="char.id" 
          :character="char" 
          @click-character="selectedCharacter = $event" 
        />
      </div>

      <div v-else class="text-center py-20">
        <p class="text-slate-500 text-xl font-mono">No life forms found here...</p>
      </div>
    </main>

    <nav v-if="characters.length > 0" class="fixed bottom-8 left-1/2 -translate-x-1/2 flex items-center gap-6 bg-slate-900/90 backdrop-blur-md px-8 py-4 rounded-full border border-slate-700 shadow-2xl z-40 border-b-green-500/30">
      
      <button 
        :disabled="!info.prev || isLoading" 
        @click="page--" 
        class="text-green-500 disabled:text-slate-700 font-bold uppercase text-xs tracking-widest hover:scale-110 active:scale-95 transition-all"
      >
        Prev
      </button>

      <div class="flex flex-col items-center min-w-[100px]">
        <span class="text-[10px] text-slate-500 font-bold uppercase tracking-widest">Portal Page</span>
        <span class="text-lg font-black font-mono text-white tracking-tighter">{{ page }} / {{ Math.min(info.pages, MAX_PAGES) }}</span>
      </div>

      <button 
        :disabled="!info.next || isLoading || page >= MAX_PAGES" 
        @click="page++" 
        class="text-green-500 disabled:text-slate-700 font-bold uppercase text-xs tracking-widest hover:scale-110 active:scale-95 transition-all"
      >
        Next
      </button>

    </nav>

    <Transition name="fade">
      <CharacterModal 
        v-if="selectedCharacter" 
        :character="selectedCharacter" 
        @close="selectedCharacter = null" 
      />
    </Transition>
  </div>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>