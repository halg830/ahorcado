<script setup>
import { ref, computed } from "vue";
import inicio from "/src/assets/inicio.png";
import error1 from "/src/assets/error1.png";
import error2 from "/src/assets/error2.png";
import error3 from "/src/assets/error3.png";
import error4 from "/src/assets/error4.png";
import error5 from "/src/assets/error5.png";
import error6 from "/src/assets/error6.png";
import imgPerdiste from "/src/assets/gameOver.jpg";

const alfabeto = Array.from({ length: 26 }, (_, index) =>
  String.fromCharCode(65 + index)
);
const palabra = "verde";
const descubiertas = ref([""]);
const imgError = [inicio, error1, error2, error3, error4, error5, error6];
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

    confirmacion = true;
  }

  if (confirmacion === true) return "Ganaste";

  return "Nada";
});
</script>

<template>
  <div class="contenedor">
    <div class="parte1">
        <img id="imagen" :src="errores <= 6 ? imgError[errores] : imgPerdiste" alt="" />
      <div>
        <div class="palabra">
          <div v-for="(letra, index) in palabra" :key="index" class="letra">
            <div class="cuadrito">
              <h1 class="tamaño">{{ buscar(letra) }}</h1>
            </div>
            <hr/>
          </div>
        </div>
      </div>
    </div>
    <div class="parte2">
      <button
        v-for="(item, index) in alfabeto"
        :key="index"
        @click="comprobar(item)"
        :disabled="errores > 6 || completado === 'Ganaste'"
      >
        {{ item }}
      </button>
    </div>
    <!-- <h1>hola: {{completado}}</h1> -->
  </div>
</template>

<style scoped>
.contenedor {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.parte1 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 60%;
  width: 100%;
}
#imagen{
  width: 50vh;
  height: 50vh;
  margin: 0%;
}
.parte2 {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 40%;
  width: 100%;
}
.palabra {
  display: flex;
}

.tamaño{
  margin: 0;
  font-size: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.letra {
  margin: 20px;
}

.cuadrito {
  width: 60px;
  height: 60px;
  border: 1px solid black;
  display: flex;
  justify-content: center;
}
</style>
