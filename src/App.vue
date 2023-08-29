<script setup>
 
</script>

<template>
    <div id="contenedorTodo">
        <div class="parteuno">
            <div class="titulo"><h1>AHORCADO</h1></div>
            <a href="../juego.html">Empezar</a> 
        </div>
    </div>
</template>

<style scoped>
.titulo{
    font-size: 40px;
    font-family: 'Press Start 2P', cursive;
    background: linear-gradient(yellow,red);
    color: transparent;
    background-clip: text;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    animation: text 5s linear infinite;
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
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.399);
    height: 100%;
}
</style>