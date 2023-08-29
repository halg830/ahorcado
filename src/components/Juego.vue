<script setup>
import {ref, computed} from 'vue'
import descarga from '/src/assets/descarga.png'
import error1 from '/src/assets/error1.png'
import error2 from '/src/assets/error2.png'

const alfabeto = Array.from({ length: 26 }, (_, index) => String.fromCharCode(65 + index))

const palabra = "verde"

const descubiertas = ref([""])

const imgError = [descarga, error1, error2]

const errores = ref(0)

const comprobar = (letra)=>{
  if(descubiertas.value.includes(letra)) return

  if(palabra.includes(letra.toLowerCase())){
    descubiertas.value.push(letra)
    event.target.setAttribute("disabled", "true")
    return
  }

  errores.value+=1
}

const buscar = computed(()=>{
  return (letra)=>{
    const prueba = descubiertas.value.find(e=> String(e)===String(letra.toUpperCase()))
      if(prueba) return letra
      return ""
  } 
})


</script>

<template>
  <div>
    <div>
      <div class="palabra">
        <div v-for="(letra, index) in palabra" :key="index" class="letra">
          <div class="cuadrito">
            <h1 >{{buscar(letra) }}</h1>
          </div>

          <hr>
        </div>
      </div>

      <div>
        <button v-for="(item, index) in alfabeto" :key="index" @click="comprobar(item)">{{ item }}</button>
      </div>
    </div>
    <div>
      <img :src="imgError[errores]" alt="">
    </div>
  </div>
</template>

<style scoped>
.palabra {
  display: flex;
}

.letra {
  margin: 10px;
}

.cuadrito {
  width: 20px;
  height: 20px;
  border: 1px solid black;
}
</style>