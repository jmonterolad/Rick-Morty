<script setup>
defineProps(['character'])
defineEmits(['click-character'])
</script>

<template>
  <article 
    @click="$emit('click-character', character)"
    class="bg-slate-800 border border-slate-700 rounded-2xl overflow-hidden cursor-pointer hover:border-green-500 hover:scale-[1.03] transition-all duration-300 shadow-xl group"
  >
    <div class="relative overflow-hidden">
      <img :src="character.image" :alt="character.name" class="w-full h-64 object-cover group-hover:scale-110 transition-transform duration-500" />
      <div class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-slate-900 p-4">
        <h3 class="text-xl font-bold text-white truncate">{{ character.name }}</h3>
      </div>
    </div>
    <div class="p-4 flex items-center gap-2">
      <span :class="[
        'h-3 w-3 rounded-full',
        character.status === 'Alive' ? 'bg-green-500' : character.status === 'Dead' ? 'bg-red-500' : 'bg-gray-500'
      ]"></span>
      <span class="text-sm text-slate-300 font-medium">
        {{ character.status }} — {{ character.species }}
      </span>
    </div>
  </article>
</template>