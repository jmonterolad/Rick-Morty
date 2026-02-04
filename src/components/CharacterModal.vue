<script setup lang="ts">
defineProps<{
  character: {
    name: string;
    image: string;
    status: string;
    species: string;
    gender: string;
    origin: { name: string };
    location: { name: string };
    episode: string[];
  }
}>();

defineEmits(['close']);
</script>

<template>
  <div 
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-md"
    @click.self="$emit('close')"
  >
    <div class="bg-slate-800 border border-slate-700 w-full max-w-2xl rounded-3xl overflow-hidden shadow-[0_0_50px_rgba(0,0,0,0.5)] relative animate-modal">
      <button @click="$emit('close')" class="absolute top-4 right-6 text-slate-400 hover:text-white text-4xl z-10 transition-colors">&times;</button>
      
      <div class="flex flex-col md:flex-row">
        <img :src="character.image" class="w-full md:w-1/2 object-cover h-80 md:h-auto border-r border-slate-700" />
        <div class="p-8 flex-1">
          <h2 class="text-3xl font-black text-green-400 mb-4 leading-tight uppercase">{{ character.name }}</h2>
          <div class="space-y-4">
            <div class="group">
              <span class="text-slate-500 text-[10px] font-bold uppercase tracking-[0.2em] group-hover:text-green-500 transition-colors">Last known location:</span>
              <p class="text-slate-200 text-lg font-medium">{{ character.location.name }}</p>
            </div>
            <div class="group">
              <span class="text-slate-500 text-[10px] font-bold uppercase tracking-[0.2em] group-hover:text-green-500 transition-colors">Origin:</span>
              <p class="text-slate-200 text-lg font-medium">{{ character.origin.name }}</p>
            </div>
            <div class="pt-6 border-t border-slate-700">
              <div class="bg-slate-900/50 rounded-lg p-3 inline-block">
                <p class="text-green-500 font-mono text-sm uppercase font-bold">Files found in {{ character.episode.length }} episodes</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes modal-in {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
.animate-modal {
  animation: modal-in 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}
</style>