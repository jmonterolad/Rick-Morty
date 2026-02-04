<script setup lang="ts">
const props = defineProps<{
  status: string;
  gender: string;
}>();

const emit = defineEmits(['update:status', 'update:gender']);

const statusOptions = ['Alive', 'Dead', 'Unknown'];
const genderOptions = ['Female', 'Male', 'Genderless', 'Unknown'];
</script>

<template>
  <div class="flex flex-wrap justify-center gap-4 mt-6">
    <div class="flex flex-col">
      <label class="text-[10px] text-slate-500 font-bold uppercase mb-1 ml-2">Status</label>
      <select 
        :value="status" 
        @change="e => emit('update:status', (e.target as HTMLSelectElement).value)"
        class="bg-slate-800 border border-slate-700 text-sm rounded-xl px-4 py-2 outline-none focus:ring-2 focus:ring-green-500 transition-all text-slate-200"
      >
        <option value="">All Statuses</option>
        <option v-for="opt in statusOptions" :key="opt" :value="opt.toLowerCase()">{{ opt }}</option>
      </select>
    </div>

    <div class="flex flex-col">
      <label class="text-[10px] text-slate-500 font-bold uppercase mb-1 ml-2">Gender</label>
      <select 
        :value="gender" 
        @change="e => emit('update:gender', (e.target as HTMLSelectElement).value)"
        class="bg-slate-800 border border-slate-700 text-sm rounded-xl px-4 py-2 outline-none focus:ring-2 focus:ring-green-500 transition-all text-slate-200"
      >
        <option value="">All Genders</option>
        <option v-for="opt in genderOptions" :key="opt" :value="opt.toLowerCase()">{{ opt }}</option>
      </select>
    </div>
  </div>
</template>