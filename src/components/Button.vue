<script setup>
  const props = defineProps({
    operator: String,
    fn: Function
  })
</script>

<template>
  <button 
      class="h-10 w-10 flex justify-center items-center font-bold bg-lime-500 rounded-full hover:outline-none hover:ring-2 hover:ring-offset-2 hover-lime-500 text-white text-2xl"
      @click="fn"
      
      >{{operator}}</button>
</template>