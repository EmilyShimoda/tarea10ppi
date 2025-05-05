<script>
  import { onDestroy } from 'svelte';

  let minutes = 0;
  let seconds = 0;
  let totalSeconds = 0;
  let interval;
  let hide = true;

  function startTimer() {
    // Convertir minutos y segundos a segundos totales
    totalSeconds = minutes * 60 + seconds;

    // Limpiar cualquier intervalo anterior
    clearInterval(interval);

    // Iniciar la cuenta regresiva
    interval = setInterval(() => {
      if (totalSeconds > 0) {
        totalSeconds--;
      } else {
        clearInterval(interval);
        alert("¡Tiempo terminado!");
      }
    }, 1000);
    hide = true;
  }

  function resetTimer() {
    clearInterval(interval);
    totalSeconds = 0;
    minutes = 0;
    seconds = 0;
  }

  function open(){
    hide = false;
  }


  function close(){
    hide = true;
  }

  // Limpieza del intervalo cuando el componente se destruye
  onDestroy(() => {
    clearInterval(interval);
  });

  // Función para convertir totalSeconds de nuevo a minutos y segundos
  $: displayMinutes = Math.floor(totalSeconds / 60);
  $: displaySeconds = totalSeconds % 60;
</script>

<style>
  input {
    width: 60px;
    padding: 5px;
    margin: 5px;
    font-size: 1.2rem;
  }
/* 
  button {
    margin: 5px;
    padding: 10px 15px;
    font-size: 1rem;
    cursor: pointer;
  } */

  /* .timer {
    font-size: 2rem;
    margin-top: 20px;
    font-family: monospace;
  } */

  .hidden {
    transform: scale(0);
  }
</style>

<div class="font-mono text-xl bg-[url('/bg.jpg')] bg-center bg-cover h-screen w-screen flex justify-center items-center">
  <div class="bg-white h-[40%] w-[60%] rounded-xl flex flex-col justify-center items-center gap-5">
    <h2 class="text-2xl">Temporizador</h2>

    <div class="text-5xl ">
      {String(displayMinutes).padStart(2, '0')}:{String(displaySeconds).padStart(2, '0')}
    </div>

    <div class="flex flex-row gap-5">
      <button class="bg-yellow-200 rounded-3xl px-13 py-2" on:click={open}>Iniciar</button>
      <button class="bg-gray-300 rounded-3xl px-10 py-2" on:click={resetTimer}>Resetear</button>
    </div>

  </div>

  <div class="{hide ? "hidden" : "show"} text-xl absolute h-screen w-screen bg-[#00000085] flex justify-center items-center">
    <div class="bg-white rounded-3xl h-[40%] w-[50%] flex flex-col items-center justify-center gap-7">
      <div>
        <input type="number" bind:value={minutes} min="0" placeholder="Min" /> min 
        <br/>
        <input class="" type="number" bind:value={seconds} min="0" max="59" placeholder="Seg"/> seg
      </div>

      <div class="flex flex-row gap-5">
        <button class="bg-yellow-200 rounded-3xl px-13 py-2" on:click={startTimer}>Iniciar</button>
        <button class="bg-gray-300 rounded-3xl px-10 py-2" on:click={close}>Cancelar</button>
      </div>

    </div>
  </div>
</div>
