<script setup>
import imgAnimales from "/src/assets/animales.avif";
import imgColores from "/src/assets/colores.jpg";
import imgFrutas from "/src/assets/frutas.avif";
import { ref, computed, onMounted } from "vue";
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
import imgGanar from "/src/assets/ganar.jpg";

const app = ref(true);

const alfabeto = Array.from({ length: 26 }, (_, index) =>
  String.fromCharCode(65 + index)
);

const cambioPreguntaModal = ref(true); //true: primera pregunta, false: segunda

const data = ref({
  categoria: "Animales",
  dificultad: "Fácil",
});

const bancoPalabras = {
  Animales: {
    Fácil: ["gato", "perro", "pato", "pez"],
    Medio: ["elefante", "jirafa", "tigre", "rinoceronte"],
    Difícil: ["hipopotamo", "cocodrilo", "pinguino", "murcielago"],
  },
  Colores: {
    Fácil: ["verde", "rojo", "azul", "amarillo"],
    Medio: ["naranja", "violeta", "rosado", "gris"],
    Difícil: ["turquesa", "beige", "magenta"],
  },
  Frutas: {
    Fácil: ["manzana", "uva", "pera", "fresa"],
    Medio: ["sandia", "naranja", "platano", "kiwi"],
    Difícil: ["granada", "mango", "papaya", "frambuesa"],
  },
};

const obtenerNumero = () => Math.floor(Math.random() * 3);

const palabra = ref([]);

const armarPalabra = () => {
  console.log(data.value);
  const strPalabra =
    bancoPalabras[data.value.categoria][data.value.dificultad][obtenerNumero()];
  const arrPalabra = Array.from(strPalabra);
  if (palabra.value.length > 0) palabra.value = [];
  for (const letra of arrPalabra) {
    palabra.value.push(letra);
  }
};

const descubiertas = ref([""]);
const imgError = [
  inicio,
  error1,
  error2,
  error3,
  error4,
  error5,
  error6,
  error7,
  error8,
  error9,
];
const errores = ref(0);

const cantErrorDificultad = {
  Fácil: 1,
  Medio: 2,
  Difícil: 3,
};

const comprobar = (letra) => {
  console.log("principio");

  if (palabra.value.includes(letra.toLowerCase())) {
    console.log(
      descubiertas.value.includes(letra),
      "data",
      data.value.dificultad != "Difícil"
    );
    if (
      descubiertas.value.includes(letra) &&
      data.value.dificultad === "Difícil"
    ) {
      console.log("hola");
      errores.value += cantErrorDificultad[data.value.dificultad];
      return;
    }
    console.log("adios");
    descubiertas.value.push(letra);
    if (data.value.dificultad != "Difícil")
      event.target.setAttribute("disabled", "true");
    return;
  }

  console.log("sin if");
  errores.value += cantErrorDificultad[data.value.dificultad];
  if (data.value.dificultad != "Difícil")
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

  for (const letra of palabra.value) {
    if (!descubiertas.value.includes(letra.toUpperCase())) {
      confirmacion = false;
      break;
    }

    confirmacion = true;
  }

  if (confirmacion === true) return "Ganaste";

  return "Nada";
});

const volverJugar = () => {
  formatear()
  icon.value = true;
};

//////

const icon = ref(false);

const categorias = [
  {
    nombre: "Animales",
    imagen: imgAnimales,
  },
  {
    nombre: "Colores",
    imagen: imgColores,
  },
  {
    nombre: "Frutas",
    imagen: imgFrutas,
  },
];

const dificultades = ["Fácil", "Medio", "Difícil"];

const formatear = () => {
  data.value = {
    categoria: "",
    dificultad: "",
  };

  cambioPreguntaModal.value = true;
};

/////////

// Inicializa una variable para los datos y un indicador para verificar si están cargados
const datos = ref([]);
const datosCargados = ref(false);

// Simula una solicitud HTTP para cargar los datos (esto puede ser una solicitud real)
const cargarDatos = async (item) => {
  // Simula una demora de 1 segundo para cargar los datos (elimina esto en tu aplicación real)
  descubiertas.value = [""];
  icon.value = false;
  cambioPreguntaModal.value = true;
  errores.value = 0;
  // Luego, asigna los datos y marca que están cargados
  datos.value = ["Dato 1", "Dato 2", "Dato 3"];
  data.value.dificultad = item;
  armarPalabra();
  app.value = false;
  datosCargados.value = true;
};

const agregarcategoria = (item) => {
  data.value.categoria = item;
  cambioPreguntaModal.value = false;
};
</script>

<script>
import { ref } from "vue";

export default {
  setup() {
    return {
      icon: ref(false),
      bar: ref(false),
      bar2: ref(false),
      toolbar: ref(false),
    };
  },
};
</script>

<template>
  <div>
    <div id="contenedorTodo" v-if="app">
      <div class="parteuno">
        <div>
          <h1 class="titulo">AHORCADO</h1>
        </div>
        <button
          class="boton"
          @click="
            icon = true;
            formatear();
          "
        >
          <span>Empezar</span>
        </button>
      </div>
    </div>

    <div class="contenedor" v-if="!app">
      <div class="parte1">
        <img
          id="imagen"
          :src="
            completado === 'Ganaste'
              ? imgGanar
              : errores <= 9
              ? imgError[errores]
              : imgPerdiste
          "
          alt=""
        />
        <button
          class="volver"
          :style="errores >= 10 || completado === 'Ganaste' ? 'display: block' : 'display: none'"
          @click="() => volverJugar()"
        >
          Volver a jugar
        </button>
        <div>
          <div class="palabra">
            <div v-for="(letra, index) in palabra" :key="index" class="letra">
              <div class="cuadrito">
                <h1 class="tamaño">{{ buscar(letra) }}</h1>
              </div>
              <hr />
            </div>
          </div>
        </div>
      </div>
      <div class="contenedor2">
        <div class="parte2">
          <button
            id="buton"
            v-for="(item, index) in alfabeto"
            :key="index"
            @click="comprobar(item)"
            :disabled="errores > 9 || completado === 'Ganaste'"
          >
            {{ item }}
          </button>
        </div>
      </div>
    </div>

    <div class="q-pa-md q-gutter-sm">
      <q-dialog v-model="icon">
        <q-card class="modal">
          <q-card-section class="row items-center q-pb-none">
            <div class="text-h6">
              <p>{{
                data.categoria === ""
                  ? "Elije la categoría"
                  : "Elije la dificultad"
              }}</p>
              
            </div>
            <q-space />
            <q-btn icon="X" flat round dense v-close-popup />
          </q-card-section>

          <q-card-section class="row selecionCardCategoria">
            <div
              v-for="(item, index) in categorias"
              :key="index"
              class="cardCategoria"
              v-if="cambioPreguntaModal"
              @click="agregarcategoria(item.nombre)"
            >
              <img :src="item.imagen" alt="" class="imgsCategoria" />
              <h4>{{ item.nombre }}</h4>
            </div>

            <button
              v-for="(item, i) in dificultades"
              :key="i"
              v-else
              @click="() => cargarDatos(item)"
              class="butoncategorias"
              :class=" i==0 ? 'facil' : i==1 ? 'medio' : 'dificil'"
            >
              {{ item }}
            </button>
          </q-card-section>
        </q-card>
      </q-dialog>
    </div>
  </div>
</template>

<style scoped>

.facil:hover{
  background-color: rgb(158, 185, 255);
  box-shadow: 0px 0px 10px 0px rgb(158, 185, 255);
  border: 0px;
  border-radius: 10px;
}

.medio:hover{
  background-color: rgb(255, 146, 74);
  box-shadow: 0px 0px 10px 0px rgb(255, 146, 74);
  border: 0px;
  border-radius: 10px;
}

.dificil:hover{
  background-color: rgb(255, 70, 70);
  box-shadow: 0px 0px 10px 0px rgb(255, 70, 70);
  border: 0px;
  border-radius: 10px;
}

.selecionCardCategoria {
  display: flex;
  justify-content: center;
  align-items: center;
}

.titulo {
  font-size: 6vw;
  font-family: "Press Start 2P", cursive;
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
  font-family: "Roboto", sans-serif;
  font-size: 20px;
  font-weight: 500;
  border: none;
  cursor: pointer;
  text-transform: uppercase;
  transition: 0.3s ease all;
  border-radius: 5px;
  position: relative;
  overflow: hidden;
}

.boton span {
  position: relative;
  z-index: 2;
  transition: 0.3s ease all;
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
  transition: 0.3s ease-in-out all;
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
  margin: 10px 10px;
  border: 1px solid black;
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 25vh;
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
  align-items: center;
}

.parte1 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 10px;
  width: 100%;
  height: 70%;
  background: rgba(0, 0, 0, 0.551);
}
#imagen {
  width: 50%;
  height: 75%;
  margin: 0%;
}

.contenedor2 {
  height: 100%;
  display: flex;
  justify-content: center;
  align-content: center;
}

.parte2 {
  display: flex;
  align-content: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1vw;
  max-width: 900px;
}
#buton{
  border-radius: 15px;
  font-size: 20px;
  width: 50px;
  height: 50px;
  cursor: pointer;
}

#buton:hover{
  background-color: rgb(211, 0, 81);
  box-shadow: 0px 0px 10px 0px rgb(255, 0, 0);
}

.palabra {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 30px;
}

.tamaño {
  margin: 0;
  font-size: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

.letra {
  margin: 1vw;
}

.cuadrito {
  width: 60px;
  height: 60px;
  border: 2px solid rgb(255, 255, 255);
  display: flex;
  justify-content: center;
}

.q-pa-md {
  padding: 0px;
}


.modal{
  background:linear-gradient(rgba(74, 192, 255, 0.769), rgba(128, 0, 255, 0.641));
  cursor: pointer;
}

.cardCategoria{
  background-color: white;
  border: 0px;
  border-radius: 10px;
}

.cardCategoria:hover{
  background-color: rgb(133, 239, 255);
  box-shadow: 0px 0px 10px 0px rgb(216, 98, 255);
  border-radius: 10px;
  size: 25px;
}

.text-h6{
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: "Press Start 2P", cursive; 
}


volverajugar{
  cursor:pointer;
}

.butoncategorias{
  background-color: rgb(255, 255, 255);
  width: 150px;
  height: 50px;
  margin: 10px;
  border: 0px;
  border-radius: 10px;
  font-family:"Press Start 2P", cursive;
}

/* .butoncategorias:hover{
  background-color: rgb(255, 99, 99);
  box-shadow: 0px 0px 10px 0px red;
  border: 0px;
  border-radius: 10px;
} */

h4{
  font-size: 20px;
  font-family: "Press Start 2P", cursive; 
}

.volver{
  width: 150px;
  height: 30px;
  background-color: cornsilk;
  border: 0px;
  margin: 5px;
  cursor: pointer;
}

.volver:hover{
  background-color: rgb(253, 108, 55);
}

</style>
