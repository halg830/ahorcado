<script setup>
 
</script>

<template>
    <div id="contenedorTodo">
        <div class="parteuno">
            <div><h1 class="titulo">AHORCADO</h1></div>
            <button class="boton"><a href="../juego.html" style="text-decoration: none;"><span>Empezar</span></a></button>
        </div>
    </div>
</template>

<style scoped>

/* *{
    margin: 0;
} */
.titulo{
    font-size: 6vw;
    font-family: 'Press Start 2P', cursive;
    background: linear-gradient(yellow,red);
    color: transparent;
    background-clip: text;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    animation: text 5s linear infinite;
}

a{
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
    100%{
        filter: hue-rotate(360deg);
    }
}

.parteuno{
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
</style>