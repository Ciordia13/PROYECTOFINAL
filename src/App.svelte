<script>
  import * as d3 from "d3"
  import { onMount, onDestroy } from "svelte";

  function energiaToY(energia) {
    return 260 - (energia - 1) * 50;
  }
  function calcularAnchoSVG(datos) {
  const ancho = datos.length * 120 + 200;
  return Math.max(ancho, window.innerWidth); // nunca menor al viewport
}
  function calcularAlturaSeccion(ancho) {
  return ancho > window.innerWidth ? '500vh' : '150vh';
}
  

  let viernes = [
    {"persona": 1, "minutos": 2, "genero": "Electronica", "actividad": "semicorchea", "energia": 5},
    {"persona": 2, "minutos": 4, "genero": "Reggeaton", "actividad": "corchea", "energia": 4},
    {"persona": 3, "minutos": 2, "genero": "Otro", "actividad": "corchea", "energia": 3},
    {"persona": 4, "minutos": 4, "genero": "Cumbia", "actividad": "corchea", "energia": 5},
    {"persona": 5, "minutos": 4, "genero": "Reggeaton", "actividad": "corchea", "energia": 4},
    {"persona": 6, "minutos": 0.25, "genero": "Rock", "actividad": "blanca", "energia": 3},
    {"persona": 7, "minutos": 0.25, "genero": "Rock", "actividad": "blanca", "energia": 3},
    {"persona": 8, "minutos": 0.25, "genero": "Pop", "actividad": "semicorchea", "energia": 5},
    {"persona": 9, "minutos": 1, "genero": "Otro", "actividad": "semicorchea", "energia": 4},
    {"persona": 10, "minutos": 2, "genero": "Rock", "actividad": "semicorchea", "energia": 4},
    {"persona": 11, "minutos": 2, "genero": "Rock", "actividad": "semicorchea", "energia": 5},
    {"persona": 12, "minutos": 1, "genero": "Reggeaton", "actividad": "blanca", "energia": 4},
    {"persona": 13, "minutos": 0.5, "genero": "Reggeaton", "actividad": "blanca", "energia": 5},
    {"persona": 14, "minutos": 4, "genero": "Rock", "actividad": "blanca", "energia": 4},
    {"persona": 15, "minutos": 2, "genero": "Cumbia", "actividad": "blanca", "energia": 3},
    {"persona": 16, "minutos": 1, "genero": "Otro", "actividad": "blanca", "energia": 2},
    {"persona": 17, "minutos": 1, "genero": "Cumbia", "actividad": "redonda", "energia": 5},
    {"persona": 18, "minutos": 1, "genero": "Reggeaton", "actividad": "redonda", "energia": 3},
    {"persona": 19, "minutos": 4, "genero": "Pop", "actividad": "blanca", "energia": 5},
    {"persona": 20, "minutos": 4, "genero": "Rock", "actividad": "redonda", "energia": 4},
    {"persona": 21, "minutos": 1, "genero": "Rock", "actividad": "blanca", "energia": 4},
    {"persona": 22, "minutos": 1, "genero": "Rock", "actividad": "blanca", "energia": 3},
    {"persona": 23, "minutos": 4, "genero": "Reggeaton", "actividad": "corchea", "energia": 4},
    {"persona": 24, "minutos": 4, "genero": "Reggeaton", "actividad": "negra", "energia": 4}
  ];
  let sabado = [
    {"persona": 1, "minutos": 4, "genero": "Cumbia", "actividad": "blanca", "energia": 3},
    {"persona": 2, "minutos": 2, "genero": "Cumbia", "actividad": "redonda", "energia": 3},
    {"persona": 3, "minutos": 0.5, "genero": "Cumbia", "actividad": "redonda", "energia": 4},
    {"persona": 4, "minutos": 4, "genero": "Cumbia", "actividad": "corchea", "energia": 5},
    {"persona": 5, "minutos": 0.5, "genero": "Rock", "actividad": "redonda", "energia": 3},
    {"persona": 6, "minutos": 4, "genero": "Cumbia", "actividad": "corchea", "energia": 5},
    {"persona": 7, "minutos": 0.5, "genero": "Reggeaton", "actividad": "semicorchea", "energia": 4},
    {"persona": 8, "minutos": 0.5, "genero": "Cumbia", "actividad": "corchea", "energia": 5},
    {"persona": 9, "minutos": 1, "genero": "Otro", "actividad": "semicorchea", "energia": 4},
    {"persona": 10, "minutos": 2, "genero": "Rock", "actividad": "semicorchea", "energia": 4},
    {"persona": 11, "minutos": 1, "genero": "Otro", "actividad": "negra", "energia": 4},
    {"persona": 12, "minutos": 1, "genero": "Reggeaton", "actividad": "blanca", "energia": 3},
    {"persona": 13, "minutos": 0.25, "genero": "Rock", "actividad": "semicorchea", "energia": 3},
    {"persona": 14, "minutos": 1, "genero": "Rock", "actividad": "semicorchea", "energia": 5},
    {"persona": 15, "minutos": 2, "genero": "Cumbia", "actividad": "blanca", "energia": 3},
    {"persona": 16, "minutos": 0.5, "genero": "Otro", "actividad": "redonda", "energia": 1},
    {"persona": 17, "minutos": 0.25, "genero": "Otro", "actividad": "corchea", "energia": 3},
    {"persona": 18, "minutos": 2, "genero": "Reggeaton", "actividad": "corchea", "energia": 4},
    {"persona": 19, "minutos": 4, "genero": "Reggeaton", "actividad": "blanca", "energia": 5},
    {"persona": 20, "minutos": 2, "genero": "Reggeaton", "actividad": "corchea", "energia": 3},
    {"persona": 21, "minutos": 2, "genero": "Rock", "actividad": "redonda", "energia": 4},
    {"persona": 22, "minutos": 1, "genero": "Electronica", "actividad": "blanca", "energia": 4},
    {"persona": 23, "minutos": 4, "genero": "Pop", "actividad": "corchea", "energia": 4},
    {"persona": 24, "minutos": 4, "genero": "Reggeaton", "actividad": "negra", "energia": 4}
  ];
  let domingo = [
    {"persona": 1, "minutos": 0.5, "genero": "Pop", "actividad": "blanca", "energia": 2},
    {"persona": 2, "minutos": 1, "genero": "Rock", "actividad": "negra", "energia": 2},
    {"persona": 3, "minutos": 0.25, "genero": "Rock", "actividad": "redonda", "energia": 3},
    {"persona": 4, "minutos": 4, "genero": "Cumbia", "actividad": "corchea", "energia": 5},
    {"persona": 5, "minutos": 0.5, "genero": "Rock", "actividad": "blanca", "energia": 3},
    {"persona": 6, "minutos": 0.25, "genero": "Cumbia", "actividad": "redonda", "energia": 4},
    {"persona": 7, "minutos": 0.25, "genero": "Pop", "actividad": "redonda", "energia": 3},
    {"persona": 8, "minutos": 1, "genero": "Rock", "actividad": "redonda", "energia": 5},
    {"persona": 9, "minutos": 1, "genero": "Pop", "actividad": "semicorchea", "energia": 4},
    {"persona": 10, "minutos": 4, "genero": "Rock", "actividad": "blanca", "energia": 5},
    {"persona": 11, "minutos": 2, "genero": "Rock", "actividad": "semicorchea", "energia": 5},
    {"persona": 12, "minutos": 1, "genero": "Reggeaton", "actividad": "blanca", "energia": 3},
    {"persona": 13, "minutos": 1, "genero": "Rock", "actividad": "negra", "energia": 5},
    {"persona": 14, "minutos": 0.25, "genero": "Reggeaton", "actividad": "redonda", "energia": 1},
    {"persona": 15, "minutos": 2, "genero": "Cumbia", "actividad": "blanca", "energia": 3},
    {"persona": 16, "minutos": 0.25, "genero": "Electronica", "actividad": "redonda", "energia": 1},
    {"persona": 17, "minutos": 0.5, "genero": "Rock", "actividad": "blanca", "energia": 3},
    {"persona": 18, "minutos": 2, "genero": "Rock", "actividad": "blanca", "energia": 3},
    {"persona": 19, "minutos": 2, "genero": "Pop", "actividad": "redonda", "energia": 5},
    {"persona": 20, "minutos": 2, "genero": "Rock", "actividad": "negra", "energia": 3},
    {"persona": 21, "minutos": 0.5, "genero": "Rock", "actividad": "redonda", "energia": 3},
    {"persona": 22, "minutos": 0.5, "genero": "Rock", "actividad": "redonda", "energia": 4},
    {"persona": 23, "minutos": 2, "genero": "Rock", "actividad": "redonda", "energia": 2},
    {"persona": 24, "minutos": 4, "genero": "Pop", "actividad": "negra", "energia": 4}
  ];

  // Escala de color por género (puede ser categórica)
  const colorScale = d3.scaleOrdinal()
    .domain(["Rock", "Reggeaton", "Cumbia", "Electronica", "Pop", "Otro"])
    .range(["hsl(24, 95%, 50%)",    // Rock → rojo
    "hsl(240, 100%, 50%)",  // Reggeaton → azul
    "hsl(116, 100%, 50%)",  // Cumbia → verde hoja
    "hsl(290, 80%, 50%)",   // Electronica → violeta orquídea
    "hsl(330, 100%, 55%)",   // Pop → rosa estable
    "hsl(195, 100%, 55%)"    // Otro → celeste
  ]);

  // Escala de brillo (cuantitativa)
  const brilloScale = d3.scaleLinear()
  .domain([0.25, 0.5, 1, 2, 4]) // minutos escuchados
  .range([75, 65, 50, 40, 30]); // luminosidad en %

  // Combinar las dos escalas: género + minutos
  function colorFinal(genero, minutos) {
    const baseColor = d3.color(colorScale(genero));
    const hsl = d3.hsl(baseColor); // convierte a HSL
    hsl.l = brilloScale(minutos) / 100; // ajustar brillo
    return hsl.formatHsl(); // devuelve string "hsl(H, S%, L%)"
  }

  // Filtros: solo uno activo por vez
  let filtroNotas = null;
  let filtroEnergia = null;
  let filtroTiempo = null;
  let filtroGenero = null;

  function toggleFiltroNotas(valor) {
    filtroNotas = filtroNotas === valor ? null : valor;

  }
  function toggleFiltroEnergia(valor) {
    filtroEnergia = filtroEnergia === valor ? null : valor;

  }
  function toggleFiltroTiempo(valor) {
    filtroTiempo = filtroTiempo === valor ? null : valor;
   
  }
  function toggleFiltroGenero(valor) {
    filtroGenero = filtroGenero === valor ? null : valor;
    
  }

function limpiarFiltros() {
  filtroNotas = null;
  filtroEnergia = null;
  filtroTiempo = null;
  filtroGenero = null;
}


function filtrarGlobal(datos, notas, energia, tiempo, genero) {
  return datos.filter(d => {
    const notasOk = notas === null || d.actividad === notas;
    const energiaOk = energia === null || d.energia === energia;
    const tiempoOk = tiempo === null || d.minutos === tiempo;
    const generoOk = genero === null || d.genero === genero;
    return notasOk && energiaOk && tiempoOk && generoOk;
  });
}

  let mostrarReferencias = false;
  let menuAbierto = false;
  let submenuFiltro = ""; // "", "notas", "energia", "tiempo", "genero"
  let menuFiltroAbierto = false;

  $: viernesFiltrado = filtrarGlobal(viernes, filtroNotas, filtroEnergia, filtroTiempo, filtroGenero);
  $: sabadoFiltrado = filtrarGlobal(sabado, filtroNotas, filtroEnergia, filtroTiempo, filtroGenero);
  $: domingoFiltrado = filtrarGlobal(domingo, filtroNotas, filtroEnergia, filtroTiempo, filtroGenero);

  let header;
  onMount(() => {
    const cerrarConEscape = (e) => {
      if (e.key === "Escape") {
        mostrarReferencias = false;
      }
    };
    window.addEventListener("keydown", cerrarConEscape);
    onDestroy(() => {
      window.removeEventListener("keydown", cerrarConEscape);
    });
  });

</script>

<main>
  <!-- HEADER principal tipo web -->
  <header class="main-header" bind:this={header}>
    <div class="logo-container">
      <img src="/images/logo.svg" alt="Logo de CanalMix" class="logo-img" />
      <span class="logo-text">CanalMix</span>
    </div>
    <nav class="nav">
      <a href="#sectionPin1" class="nav-link">Viernes</a>
      <a href="#sectionPin2" class="nav-link">Sábado</a>
      <a href="#sectionPin3" class="nav-link">Domingo</a>
    </nav>
  </header>

  <!-- PRESENTACIÓN de la visualización -->
  <section class="intro">
    <h1 class="titulo-principal">
      <span>Fin de semana musical</span>
    </h1>
    <p class="subtitulo">
      Este no es un pentagrama tradicional.<br />
      Es una memoria codificada en música.<br />
      Cada nota es un joven entrevistado:<br />
      ¿Qué música escuchó? ¿Cuánto tiempo?<br />
      ¿Qué estaba haciendo? ¿Cómo se sintió?<br />
      Para descifrar qué pasó, hay que observar con atención.<br />
      <strong>¿Te animás a leer entre líneas?</strong>
    </p>
  </section>

  <div class="leyenda">
    <img src="/images/Referencias.svg" alt="Leyenda explicativa de notas, géneros y días" />
  </div>

  <!-- Scroll horizontal Viernes  -->
  <section id="sectionPin1"  style="height: {calcularAlturaSeccion(calcularAnchoSVG(viernesFiltrado))};">
    <div class="pin-wrap-sticky">
      <div class="pin-wrap"  style = "width: {calcularAnchoSVG(viernesFiltrado)}px;">
        <div class="titulo-con-filtro">
          <h2>Viernes</h2>
          <div class="contenedor-filtro">
            <button class="boton-filtro-dia" on:click={() => menuFiltroAbierto = !menuFiltroAbierto}>
              <i class="fa-solid fa-filter"></i>
            </button>
            {#if menuFiltroAbierto}
              <div class="menu-filtros-dia">
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "notas" ? "" : "notas"}>Notas</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "energia" ? "" : "energia"}>Energía</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "tiempo" ? "" : "tiempo"}>Tiempo</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "genero" ? "" : "genero"}>Género</button>
                <button class="nav-link limpiar-filtros" on:click={limpiarFiltros}>Limpiar</button>
              </div>
              <!-- Submenú Notas -->
              {#if submenuFiltro === "notas"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroNotas === 'semicorchea' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('semicorchea')}>Entrenando</button>
                  <button class="nav-link {filtroNotas === 'corchea' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('corchea')}>Fiesta</button>
                  <button class="nav-link {filtroNotas === 'negra' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('negra')}>Estudiando</button>
                  <button class="nav-link {filtroNotas === 'blanca' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('blanca')}>Viajando</button>
                  <button class="nav-link {filtroNotas === 'redonda' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('redonda')}>Otra cosa</button>
                </div>
              {/if}
              <!-- Submenú Energía -->
              {#if submenuFiltro === "energia"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroEnergia === 1 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(1)}>Nada</button>
                  <button class="nav-link {filtroEnergia === 2 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(2)}>Poco</button>
                  <button class="nav-link {filtroEnergia === 3 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(3)}>Regular</button>
                  <button class="nav-link {filtroEnergia === 4 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(4)}>Bastante</button>
                  <button class="nav-link {filtroEnergia === 5 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(5)}>Mucho</button>
                </div>
              {/if}
              <!-- Submenú Tiempo -->
              {#if submenuFiltro === "tiempo"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroTiempo === 0.25 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(0.25)}>15m</button>
                  <button class="nav-link {filtroTiempo === 0.5 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(0.5)}>30m</button>
                  <button class="nav-link {filtroTiempo === 1 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(1)}>1h</button>
                  <button class="nav-link {filtroTiempo === 2 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(2)}>2h</button>
                  <button class="nav-link {filtroTiempo === 4 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(4)}>4h</button>
                </div>
              {/if}
              <!-- Submenú Género -->
              {#if submenuFiltro === "genero"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroGenero === 'Rock' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Rock')}>Rock</button>
                  <button class="nav-link {filtroGenero === 'Reggeaton' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Reggeaton')}>Reggeaton</button>
                  <button class="nav-link {filtroGenero === 'Cumbia' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Cumbia')}>Cumbia</button>
                  <button class="nav-link {filtroGenero === 'Electronica' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Electronica')}>Electronica</button>
                  <button class="nav-link {filtroGenero === 'Pop' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Pop')}>Pop</button>
                  <button class="nav-link {filtroGenero === 'Otro' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Otro')}>Otro</button>
                </div>
              {/if}
            {/if}
          </div>
        </div>
        <svg width={calcularAnchoSVG(viernesFiltrado)} height="300">
          {#each Array(5) as _, i}
            <line 
              x1="0" 
              y1={260 - i * 50} 
              x2={calcularAnchoSVG(viernesFiltrado)} 
              y2={260 - i * 50} 
              stroke="rgba(0, 0, 0, 0.4)"
              stroke-width="2" />
          {/each}
          {#each viernesFiltrado as d, i}
            {#if d.actividad == "semicorchea"} 
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0C64.0995 18.1847 85.7828 21.2927 81.4248 45.4434C87.2557 53.528 88.6326 61.5868 78.8291 76C88.9412 49.3738 78.0588 46.0018 58 34.209V88H56V0H58ZM58.3447 16.5859C65.6117 28.9565 74.4233 36.5251 80.1182 43.7168C79.4262 36.8479 78.9017 26.9955 58.3447 16.5859Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {:else if d.actividad == "corchea"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0C74.0585 27.8643 98.1481 31.597 78.8291 60C88.9412 33.3738 78.0588 30.0018 58 18.209V88H56V0H58Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)}/>
              </svg>
            {:else if d.actividad == "negra"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 180" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0H56V88H58V0Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {:else if d.actividad == "blanca"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0H56V88H58V0Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935ZM55.417 83.1949C54.0312 81.0104 46.9762 82.2119 39.6694 85.9254C39.5434 85.9254 39.5434 86.0346 39.4174 86.0346C39.2914 86.0346 39.1654 86.1438 39.1654 86.1438C31.9845 89.8573 27.3233 94.5537 28.835 96.6289C30.2208 98.8133 37.2757 97.6119 44.5826 93.8984C44.7086 93.8984 44.7086 93.7892 44.8346 93.7892L44.9606 93.68C52.1415 90.0757 56.8027 85.3793 55.417 83.1949V83.1949Z" fill={colorFinal(d.genero, d.minutos)}/>
              </svg>
            {:else if d.actividad == "redonda"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M32.0974 100.514C26.4533 98.8054 22 94.1609 22 89.9836C22 78.1601 47.8128 73.4812 58.4729 83.3725C70.0004 94.0687 51.188 106.295 32.0974 100.514H32.0974ZM49.3147 97.5442C52.4551 92.8287 49.4526 83.486 44.0131 81.0476C36.025 77.4667 31.1285 83.5689 34.4555 92.9586C36.7567 99.4532 46.1191 102.343 49.3147 97.5442Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {/if}
          {/each}
        </svg>
      </div>
    </div>
  </section>

  <!-- ENTRE PENTAGRAMAS -->
  <section class="transicion">
    <i class="fa-solid fa-arrow-down fa-3x"></i>
  </section>

  <!-- Scroll horizontal Sabado  -->
<section id="sectionPin2" style="height: {calcularAlturaSeccion(calcularAnchoSVG(sabadoFiltrado))};">
  <div class="pin-wrap-sticky">
    <div class="pin-wrap" style="width: {calcularAnchoSVG(sabadoFiltrado)}px;">
      <div class="titulo-con-filtro">
        <h2><b>Sabado</b></h2>
        <div class="contenedor-filtro">
          <button class="boton-filtro-dia" on:click={() => menuFiltroAbierto = !menuFiltroAbierto}>
            <i class="fa-solid fa-filter"></i>
          </button>
            {#if menuFiltroAbierto}
              <div class="menu-filtros-dia">
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "notas" ? "" : "notas"}>Notas</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "energia" ? "" : "energia"}>Energía</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "tiempo" ? "" : "tiempo"}>Tiempo</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "genero" ? "" : "genero"}>Género</button>
                <button class="nav-link limpiar-filtros" on:click={limpiarFiltros}>Limpiar</button>
              </div>
              <!-- Submenú Notas -->
              {#if submenuFiltro === "notas"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroNotas === 'semicorchea' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('semicorchea')}>Entrenando</button>
                  <button class="nav-link {filtroNotas === 'corchea' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('corchea')}>Fiesta</button>
                  <button class="nav-link {filtroNotas === 'negra' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('negra')}>Estudiando</button>
                  <button class="nav-link {filtroNotas === 'blanca' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('blanca')}>Viajando</button>
                  <button class="nav-link {filtroNotas === 'redonda' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('redonda')}>Otra cosa</button>
                </div>
              {/if}
              <!-- Submenú Energía -->
              {#if submenuFiltro === "energia"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroEnergia === 1 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(1)}>Nada</button>
                  <button class="nav-link {filtroEnergia === 2 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(2)}>Poco</button>
                  <button class="nav-link {filtroEnergia === 3 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(3)}>Regular</button>
                  <button class="nav-link {filtroEnergia === 4 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(4)}>Bastante</button>
                  <button class="nav-link {filtroEnergia === 5 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(5)}>Mucho</button>
                </div>
              {/if}
              <!-- Submenú Tiempo -->
              {#if submenuFiltro === "tiempo"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroTiempo === 0.25 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(0.25)}>15m</button>
                  <button class="nav-link {filtroTiempo === 0.5 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(0.5)}>30m</button>
                  <button class="nav-link {filtroTiempo === 1 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(1)}>1h</button>
                  <button class="nav-link {filtroTiempo === 2 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(2)}>2h</button>
                  <button class="nav-link {filtroTiempo === 4 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(4)}>4h</button>
                </div>
              {/if}
              <!-- Submenú Género -->
              {#if submenuFiltro === "genero"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroGenero === 'Rock' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Rock')}>Rock</button>
                  <button class="nav-link {filtroGenero === 'Reggeaton' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Reggeaton')}>Reggeaton</button>
                  <button class="nav-link {filtroGenero === 'Cumbia' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Cumbia')}>Cumbia</button>
                  <button class="nav-link {filtroGenero === 'Electronica' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Electronica')}>Electronica</button>
                  <button class="nav-link {filtroGenero === 'Pop' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Pop')}>Pop</button>
                  <button class="nav-link {filtroGenero === 'Otro' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Otro')}>Otro</button>
                </div>
              {/if}
            {/if}
          </div>
        </div>
        <svg width={calcularAnchoSVG(sabadoFiltrado)} height="300">
          {#each Array(5) as _, i}
            <line 
              x1="0" 
              y1={260 - i * 50} 
              x2={calcularAnchoSVG(sabadoFiltrado)}
              y2={260 - i * 50} 
              stroke="rgba(0, 0, 0, 0.4)"
              stroke-width="2" />
          {/each}
          {#each sabadoFiltrado as d, i}
            {#if d.actividad == "semicorchea"} 
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0C64.0995 18.1847 85.7828 21.2927 81.4248 45.4434C87.2557 53.528 88.6326 61.5868 78.8291 76C88.9412 49.3738 78.0588 46.0018 58 34.209V88H56V0H58ZM58.3447 16.5859C65.6117 28.9565 74.4233 36.5251 80.1182 43.7168C79.4262 36.8479 78.9017 26.9955 58.3447 16.5859Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {:else if d.actividad == "corchea"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0C74.0585 27.8643 98.1481 31.597 78.8291 60C88.9412 33.3738 78.0588 30.0018 58 18.209V88H56V0H58Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)}/>
              </svg>
            {:else if d.actividad == "negra"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 180" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0H56V88H58V0Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {:else if d.actividad == "blanca"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0H56V88H58V0Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935ZM55.417 83.1949C54.0312 81.0104 46.9762 82.2119 39.6694 85.9254C39.5434 85.9254 39.5434 86.0346 39.4174 86.0346C39.2914 86.0346 39.1654 86.1438 39.1654 86.1438C31.9845 89.8573 27.3233 94.5537 28.835 96.6289C30.2208 98.8133 37.2757 97.6119 44.5826 93.8984C44.7086 93.8984 44.7086 93.7892 44.8346 93.7892L44.9606 93.68C52.1415 90.0757 56.8027 85.3793 55.417 83.1949V83.1949Z" fill={colorFinal(d.genero, d.minutos)}/>
              </svg>
            {:else if d.actividad == "redonda"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M32.0974 100.514C26.4533 98.8054 22 94.1609 22 89.9836C22 78.1601 47.8128 73.4812 58.4729 83.3725C70.0004 94.0687 51.188 106.295 32.0974 100.514H32.0974ZM49.3147 97.5442C52.4551 92.8287 49.4526 83.486 44.0131 81.0476C36.025 77.4667 31.1285 83.5689 34.4555 92.9586C36.7567 99.4532 46.1191 102.343 49.3147 97.5442Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {/if}
          {/each}
        </svg>
      </div>
    </div>
  </section>

  <!-- ENTRE PENTAGRAMAS -->
  <section class="transicion">
    <i class="fa-solid fa-arrow-down fa-3x"></i>
  </section>

  <!-- Scroll horizontal Domingo -->
<section id="sectionPin3" style="height: {calcularAlturaSeccion(calcularAnchoSVG(domingoFiltrado))};">
  <div class="pin-wrap-sticky">
    <div class="pin-wrap"  style="width: {calcularAnchoSVG(domingoFiltrado)}px;">
      <div class="titulo-con-filtro">
        <h2><b>Domingo</b></h2>
        <div class="contenedor-filtro">
          <button class="boton-filtro-dia" on:click={() => menuFiltroAbierto = !menuFiltroAbierto}>
            <i class="fa-solid fa-filter"></i>
          </button>
              {#if menuFiltroAbierto}
              <div class="menu-filtros-dia">
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "notas" ? "" : "notas"}>Notas</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "energia" ? "" : "energia"}>Energía</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "tiempo" ? "" : "tiempo"}>Tiempo</button>
                <button class="nav-link" on:click={() => submenuFiltro = submenuFiltro === "genero" ? "" : "genero"}>Género</button>
                <button class="nav-link limpiar-filtros" on:click={limpiarFiltros}>Limpiar</button>
              </div>
              <!-- Submenú Notas -->
              {#if submenuFiltro === "notas"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroNotas === 'semicorchea' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('semicorchea')}>Entrenando</button>
                  <button class="nav-link {filtroNotas === 'corchea' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('corchea')}>Fiesta</button>
                  <button class="nav-link {filtroNotas === 'negra' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('negra')}>Estudiando</button>
                  <button class="nav-link {filtroNotas === 'blanca' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('blanca')}>Viajando</button>
                  <button class="nav-link {filtroNotas === 'redonda' ? 'activo' : ''}" on:click={() => toggleFiltroNotas('redonda')}>Otra cosa</button>
                </div>
              {/if}
              <!-- Submenú Energía -->
              {#if submenuFiltro === "energia"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroEnergia === 1 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(1)}>Nada</button>
                  <button class="nav-link {filtroEnergia === 2 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(2)}>Poco</button>
                  <button class="nav-link {filtroEnergia === 3 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(3)}>Regular</button>
                  <button class="nav-link {filtroEnergia === 4 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(4)}>Bastante</button>
                  <button class="nav-link {filtroEnergia === 5 ? 'activo' : ''}" on:click={() => toggleFiltroEnergia(5)}>Mucho</button>
                </div>
              {/if}
              <!-- Submenú Tiempo -->
              {#if submenuFiltro === "tiempo"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroTiempo === 0.25 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(0.25)}>15m</button>
                  <button class="nav-link {filtroTiempo === 0.5 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(0.5)}>30m</button>
                  <button class="nav-link {filtroTiempo === 1 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(1)}>1h</button>
                  <button class="nav-link {filtroTiempo === 2 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(2)}>2h</button>
                  <button class="nav-link {filtroTiempo === 4 ? 'activo' : ''}" on:click={() => toggleFiltroTiempo(4)}>4h</button>
                </div>
              {/if}
              <!-- Submenú Género -->
              {#if submenuFiltro === "genero"}
                <div class="submenu-filtros-dia">
                  <button class="nav-link {filtroGenero === 'Rock' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Rock')}>Rock</button>
                  <button class="nav-link {filtroGenero === 'Reggeaton' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Reggeaton')}>Reggeaton</button>
                  <button class="nav-link {filtroGenero === 'Cumbia' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Cumbia')}>Cumbia</button>
                  <button class="nav-link {filtroGenero === 'Electronica' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Electronica')}>Electronica</button>
                  <button class="nav-link {filtroGenero === 'Pop' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Pop')}>Pop</button>
                  <button class="nav-link {filtroGenero === 'Otro' ? 'activo' : ''}" on:click={() => toggleFiltroGenero('Otro')}>Otro</button>
                </div>
              {/if}
            {/if}
          </div>
        </div>
        <svg width={calcularAnchoSVG(domingoFiltrado)} height="300">
          {#each Array(5) as _, i}
            <line 
              x1="0" 
              y1={260 - i * 50} 
              x2={calcularAnchoSVG(domingoFiltrado)}
              y2={260 - i * 50} 
              stroke="rgba(0, 0, 0, 0.4)"
              stroke-width="2" />
          {/each}
          {#each domingoFiltrado as d, i}
            {#if d.actividad == "semicorchea"} 
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0C64.0995 18.1847 85.7828 21.2927 81.4248 45.4434C87.2557 53.528 88.6326 61.5868 78.8291 76C88.9412 49.3738 78.0588 46.0018 58 34.209V88H56V0H58ZM58.3447 16.5859C65.6117 28.9565 74.4233 36.5251 80.1182 43.7168C79.4262 36.8479 78.9017 26.9955 58.3447 16.5859Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {:else if d.actividad == "corchea"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0C74.0585 27.8643 98.1481 31.597 78.8291 60C88.9412 33.3738 78.0588 30.0018 58 18.209V88H56V0H58Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)}/>
              </svg>
            {:else if d.actividad == "negra"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 180" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0H56V88H58V0Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {:else if d.actividad == "blanca"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M58 0H56V88H58V0Z" fill={colorFinal(d.genero, d.minutos)} />
                <path d="M27.3233 97.3935C23.7958 92.1509 27.5752 84.6147 35.638 80.5736C43.7007 76.5324 53.1493 77.297 56.6768 82.5395C60.2042 87.7821 56.4248 95.3183 48.362 99.4687C40.2993 103.51 30.8507 102.636 27.3233 97.3935V97.3935ZM55.417 83.1949C54.0312 81.0104 46.9762 82.2119 39.6694 85.9254C39.5434 85.9254 39.5434 86.0346 39.4174 86.0346C39.2914 86.0346 39.1654 86.1438 39.1654 86.1438C31.9845 89.8573 27.3233 94.5537 28.835 96.6289C30.2208 98.8133 37.2757 97.6119 44.5826 93.8984C44.7086 93.8984 44.7086 93.7892 44.8346 93.7892L44.9606 93.68C52.1415 90.0757 56.8027 85.3793 55.417 83.1949V83.1949Z" fill={colorFinal(d.genero, d.minutos)}/>
              </svg>
            {:else if d.actividad == "redonda"}
              <svg width="130" height="130" x={100 + i * 100} y={energiaToY(d.energia) - 65} viewBox="0 0 86 164" fill={colorFinal(d.genero, d.minutos)} xmlns="http://www.w3.org/2000/svg">
                <path d="M32.0974 100.514C26.4533 98.8054 22 94.1609 22 89.9836C22 78.1601 47.8128 73.4812 58.4729 83.3725C70.0004 94.0687 51.188 106.295 32.0974 100.514H32.0974ZM49.3147 97.5442C52.4551 92.8287 49.4526 83.486 44.0131 81.0476C36.025 77.4667 31.1285 83.5689 34.4555 92.9586C36.7567 99.4532 46.1191 102.343 49.3147 97.5442Z" fill={colorFinal(d.genero, d.minutos)} />
              </svg>
            {/if}
          {/each}
        </svg>
      </div>
    </div>
  </section>
    <section class="bloque-titulo">
      <h2 class="titulo-seccion">
        Explora y filtra como quieras lo que escuchó la gente el fin de semana
      </h2>
      <p class="subtitulo-seccion">
        Cada círculo representa a una persona. Agrupadas por día, actividad, tiempo, energía y género musical, exploralo como quieras.
      </p>
    </section>
    <section class="graficos-flourish">
    <div class="flourish-embed flourish-survey" data-src="visualisation/23710997">
    <script src="https://public.flourish.studio/resources/embed.js"></script>
    <noscript><img src="https://public.flourish.studio/visualisation/23710997/thumbnail" width="100%" alt="survey visualization" /></noscript>
    </div>
    </section>
    <section class="bloque-titulo">
      <h2 class="titulo-seccion">Cómo varió la energía a lo largo del fin de semana</h2>
      <p class="subtitulo-seccion">
        El sábado se destacó con un salto de energía. El viernes fue más intermedio, con variaciones suaves, y el domingo terminó con un mood mucho más tranquilo.
      </p>
    </section>
    <section class="graficos-flourish">
    <div class="flourish-embed flourish-pictogram" data-src="visualisation/23715747">
    <script src="https://public.flourish.studio/resources/embed.js"></script>
    <noscript><img src="https://public.flourish.studio/visualisation/23715747/thumbnail" width="100%" alt="pictogram visualization" /></noscript>
    </div>
  </section>

  <!-- BOTÓN flotante de ayuda -->
  <div class="boton-ayuda" on:click={() => mostrarReferencias = !mostrarReferencias} title="Ver ayuda">
    ?
  </div>
  <!-- MODAL de referencias -->
  {#if mostrarReferencias}
    <div class="overlay" on:click={() => mostrarReferencias = false}>
      <div class="modal-leyenda" on:click|stopPropagation>
        <img src="/images/Referencias.svg" alt="Leyenda explicativa de notas, géneros y días" />
      </div>
    </div>
  {/if}
  <!-- Botón flotante para menú -->
  <div class="boton-menu" on:click={() => menuAbierto = !menuAbierto} title="Navegar días">
    ☰
  </div>
  <!-- Botones de días -->
  {#if menuAbierto}
    <div class="contenedor-menu-dias">
      <a href="#sectionPin1" class="nav-link" on:click={() => menuAbierto = false}>Viernes</a>
      <a href="#sectionPin2" class="nav-link" on:click={() => menuAbierto = false}>Sábado</a>
      <a href="#sectionPin3" class="nav-link" on:click={() => menuAbierto = false}>Domingo</a>
    </div>
  {/if}

    

</main>



<style>
  /* INTRODUCCIÓN */
.intro {
  padding: 80px 20px 40px;
  text-align: center;
  margin-top: 100px;

}

.titulo-principal {
  font-size: 3rem;
  font-weight: 800;
  color: #d43f3a;
  font-family: 'Karla', sans-serif;
  margin-bottom: 20px;
}

.subtitulo {
  font-size: 1.2rem;
  font-weight: 300;
  line-height: 1.8;
  max-width: 850px;
  margin: 0 auto;
  color: #444;
}

.subtitulo strong {
  font-weight: 600;
  color: #111;
}

.leyenda {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 40px auto;
  padding: 30px;
  background-color: #888888; /* igual que el fondo del SVG */
  border-radius: 16px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
  max-width: 95%;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeInUp 1s ease-out forwards;
  animation-delay: 0.2s;
}

.leyenda img {
  max-width: 100%;
  height: auto;
  border-radius: 12px;
}


@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}




  @keyframes move {
  to {
    transform: translateX(calc(-100% + 100vw));
  }
}

#sectionPin1, #sectionPin2, #sectionPin3 {
  
  overflow: visible;

  view-timeline-axis: block;
}

#sectionPin1 {
  view-timeline-name: --section-pin-1;
}
#sectionPin2 {
  view-timeline-name: --section-pin-2;
}
#sectionPin3 {
  view-timeline-name: --section-pin-3;
}


.pin-wrap-sticky {
  position: sticky;
  top: 0;
  height: 100vh;
  /* width is set dynamically via style attribute in markup */
  
  margin-bottom: 0px; 
}

.pin-wrap {
  height: 100vh;
  will-change: transform;
  display: flex;
  flex-direction: column; /* Cambia a columna */
  align-items: center;
  justify-content: center;
  
  animation: linear move forwards;
  /* animation-timeline: --section-pin-tl; */
  animation-range: contain 0% contain 100%;
  will-change: transform;
}
#sectionPin1 .pin-wrap {
  animation: linear move forwards;
  animation-timeline: --section-pin-1;
  animation-range: contain 7% contain 100%;
}

#sectionPin2 .pin-wrap {
  animation: linear move forwards;
  animation-timeline: --section-pin-2;
  animation-range: contain 7% contain 100%;
}

#sectionPin3 .pin-wrap {
  animation: linear move forwards;
  animation-timeline: --section-pin-3;
  animation-range: contain 7% contain 100%;
}
svg {
  display: block;
  margin: 0 auto;
}
.pentagrama{
  position: relative;
  top: 20px;
  left: 20px;
  font-weight: bold;
  font-size: 1.6rem;
  color: #d43f3a;
  align-items: center;
  justify-content: space-between;
}

.logo-container {
  display: flex;
  align-items: center;
  gap: 1rem; /* Más espacio entre logo y texto */
}

.logo-img {
  width: 60px; /* Tamaño aumentado (antes era 40px) */
  height: auto;
  transition: transform 0.3s ease; /* Efecto hover opcional */
}

.logo-img:hover {
  transform: scale(1.05); /* Efecto hover opcional */
}

.logo-text {
  font-size: 1.8rem;
  font-weight: 800;
  font-style: italic; /* Esto añade la cursiva */
  color: #333;
  font-family: 'Georgia', 'Times New Roman', serif; /* Fuentes con itálicas más marcadas */
  /* Manteniendo el resto de tus estilos... */
  text-transform: uppercase;
  letter-spacing: 1px;
}

</style>


