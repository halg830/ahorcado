<script setup>
import imgAnimales from '/src/assets/animales.avif'
import imgColores from '/src/assets/colores.jpg'
import imgFrutas from '/src/assets/frutas.avif'
import { ref, computed } from "vue";
import inicio from "/src/assets/inicio.png";
import error1 from "/src/assets/error1.png";
import error2 from "/src/assets/error2.png";
import error3 from "/src/assets/error3.png";
import error4 from "/src/assets/error4.png";
import error5 from "/src/assets/error5.png";
import error6 from "/src/assets/error6.png";
import error7 from "/src/assets/error7.png";
import error8 from "/src/assets/error8.png";
import error9 from "/src/assets/error9.png";
import imgPerdiste from "/src/assets/gameover.png";

const app = ref(true)

const alfabeto = Array.from({ length: 26 }, (_, index) =>
  String.fromCharCode(65 + index)
);
const palabra = "verde";
const descubiertas = ref([""]);
const imgError = [inicio, error1, error2, error3, error4, error5, error6, error7, error8, error9,];
const errores = ref(0);

const comprobar = (letra) => {
  if (descubiertas.value.includes(letra)) return;

  if (palabra.includes(letra.toLowerCase())) {
    descubiertas.value.push(letra);
    event.target.setAttribute("disabled", "true");
    return;
  }

  errores.value += 1;
  event.target.setAttribute("disabled", "true");
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

const icon = ref(false);

const categorias = [
	{
		nombre: "Animales",
		imagen: imgAnimales
	},
	{
		nombre: "Colores",
		imagen: imgColores
	},
	{
		nombre: "Frutas",
		imagen: imgFrutas
	}
]

const dificultades = ["Fácil", "Medio", "Difícil"]

const data = ref({
	categoria: "",
	dificultad: ""
})

const formatear = ()=>{
	data.value = {
		categoria: "",
		dificultad: ""
	}
}

const comenzar=(item)=>{
	data.dificultad=item
	
	app.value = false
}
</script>

<script>
import { ref } from 'vue'

export default {
	setup() {
		return {
			icon: ref(false),
			bar: ref(false),
			bar2: ref(false),
			toolbar: ref(false)
		}
	},
	
}
</script>



<template>
	<div id="contenedorTodo" v-if="app">
		<div class="parteuno">
			<div>
				<h1 class="titulo">AHORCADO</h1>
			</div>
			<button class="boton" @click="icon = true; formatear()"><span>Empezar</span></button>
		</div>
		
		<div class="q-pa-md q-gutter-sm">
			<q-dialog v-model="icon">
				<q-card>
					<q-card-section class="row items-center q-pb-none">
						<div class="text-h6">{{ data.categoria === "" ? "Elije la categoría" : "Elije la dificultad" }}
						</div>
						<q-space />
						<q-btn icon="X" flat round dense v-close-popup />
					</q-card-section>

					<q-card-section class="row">
						<div v-for="(item, index) in categorias" :key="index" class="cardCategoria"
							v-if="data.categoria === ''" @click="data.categoria = item.nombre">
							<img :src="item.imagen" alt="" class="imgsCategoria">
							<h4>{{ item.nombre }}</h4>
						</div>

						<button v-for="(item, i) in dificultades" :key="i" v-else @click="comenzar(item)">{{ item }}</button>
					</q-card-section>
				</q-card>
			</q-dialog>
		</div>
	</div>

	<div class="contenedor" v-if="!app">
    <div class="parte1">
        <img id="imagen" :src="errores <= 9 ? imgError[errores] : imgPerdiste" alt="" />
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
        :disabled="errores > 9 || completado === 'Ganaste'"
      >
        {{ item }}
      </button>
    </div>
    <!-- <h1>hola: {{completado}}</h1> -->
  </div>
</template>

<style scoped>
/* *{
    margin: 0;
} */
.titulo {
	font-size: 6vw;
	font-family: 'Press Start 2P', cursive;
	background: linear-gradient(yellow, red);
	color: transparent;
	background-clip: text;
	-webkit-background-clip: text;
	-moz-background-clip: text;
	animation: text 5s linear infinite;
}

a {
	z-index: 5;
	width: 80%;
	display: flex;
	height: 100%;
	align-items: center;
	justify-content: center;
}

@keyframes text {
	0% {
		filter: hue-rotate(0deg);
	}

	100% {
		filter: hue-rotate(360deg);
	}
}

.parteuno {
	/* padding: 85px 0px; */
	display: flex;
	/* justify-content: space-between; */
	flex-direction: column;
	align-items: center;
	background-color: rgba(0, 0, 0, 0.399);
	height: 100%;
}

.boton {
	display: inline-flex;
	align-items: center;
	justify-content: center;
	width: 100%;
	height: 80px;
	background: black;
	color: #fff;
	font-family: 'Roboto', sans-serif;
	font-size: 20px;
	font-weight: 500;
	border: none;
	cursor: pointer;
	text-transform: uppercase;
	transition: .3s ease all;
	border-radius: 5px;
	position: relative;
	overflow: hidden;

}

.boton span {
	position: relative;
	z-index: 2;
	transition: .3s ease all;
	color: white;
}

.boton::after {
	content: "";
	width: 1px;
	height: 1px;
	background: none;
	position: absolute;
	z-index: 1;
	top: 50%;
	left: 50%;
	transition: .3s ease-in-out all;
	border-radius: 100px;
	transform-origin: center;
}

.boton:hover::after {
	transform: scale(1000);
	background: #1d5f0a;
}

.boton:hover {
	background: #141414;
	font-size: 30px;
}

.imgsCategoria {

	height: 100px;
}

.cardCategoria {
	margin: 0 10px;
	border: 1px solid black;
	padding: 10px;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

.cardCategoria:hover {
	transform: scale(1.1);
}

h4 {
	margin: 0;
}

.contenedor {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background-image: url("src/assets/fondo.jpg");
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  padding: 10px;
}

.parte1 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 10px;
  width: 100%;
  background: rgba(0, 0, 0, 0.551);
}
#imagen{
  width: 65vh;
  height: 50vh;
  margin: 0%;
}
.parte2 {
  display: grid;
    width: 100%;
    grid-template-columns: repeat(9,1fr);
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