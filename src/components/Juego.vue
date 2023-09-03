<script setup>
import { ref, computed } from "vue";
import descarga from "/src/assets/descarga.png";
import error1 from "/src/assets/error1.png";
import error2 from "/src/assets/error2.png";
import error3 from "/src/assets/error3.png";
import error4 from "/src/assets/error4.png";
import error5 from "/src/assets/error5.png";
import error6 from "/src/assets/error6.png";
import imgPerdiste from "/src/assets/gameOver.jpg"

const alfabeto = Array.from({ length: 26 }, (_, index) =>
  String.fromCharCode(65 + index)
);
const palabra = "verde";
const descubiertas = ref([""]);
const imgError = [descarga, error1, error2, error3, error4, error5, error6];
const errores = ref(0);

const comprobar = (letra) => {
  if (descubiertas.value.includes(letra)) return;

  if (palabra.includes(letra.toLowerCase())) {
    descubiertas.value.push(letra);
    event.target.setAttribute("disabled", "true");
    return;
  }

  errores.value += 1;
};

const buscar = computed(() => {
  return (letra) => {
    const prueba = descubiertas.value.find(
      (e) => String(e) === String(letra.toUpperCase())
    );
    if (prueba) {
      return letra;
    }
    return "";
  };
});

const completado = computed(() => {
  let confirmacion = false;

  for (const letra of palabra) {
    if (!descubiertas.value.includes(letra.toUpperCase())) {
      confirmacion = false;
      break;
    }
    
    confirmacion= true
  }

  if(confirmacion===true) return "Ganaste"

  return "Nada"
});
</script>

<template>
  <div>
    <div>
      <div class="palabra">
        <div v-for="(letra, index) in palabra" :key="index" class="letra">
          <div class="cuadrito">
            <h1>{{ buscar(letra) }}</h1>
          </div>

          <hr />
        </div>
      </div>

      <div>
        <button
          v-for="(item, index) in alfabeto"
          :key="index"
          @click="comprobar(item)"
          :disabled="errores>6 || completado==='Ganaste'"
        >
          {{ item }}
        </button>
      </div>
    </div>
    <div>
      <img :src="errores<=6 ? imgError[errores] : imgPerdiste" alt="" />
    </div>
    <h1>hola: {{completado}}</h1>
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
