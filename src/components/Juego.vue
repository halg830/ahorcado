<script setup>
const alfabeto = Array.from({ length: 26 }, (_, index) => String.fromCharCode(65 + index))

</script>

<template>
  <div>
    <div>
        <div>
            <button v-for="(item, index) in alfabeto" :key="index">{{ item }}</button>
        </div>
    </div>
    <div>
      
    </div>
  </div>
</template>

<style scoped>


</style>