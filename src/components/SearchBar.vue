<script setup>
let timeout = null
const emit = defineEmits(['search'])

const handleInput = (event) => {
  clearTimeout(timeout)
  timeout = setTimeout(() => {
    emit('search', event.target.value)
  }, 500)
}
</script>

<template>
  <div class="w-full max-w-md">
    <input 
      type="text" 
      @input="handleInput"
      placeholder="Search character... (e.g. Rick)"
      class="w-full px-6 py-3 rounded-full bg-slate-800 border border-slate-700 text-white focus:outline-none focus:ring-2 focus:ring-green-500 transition-all shadow-lg"
    />
  </div>
</template>