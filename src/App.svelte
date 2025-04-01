<script>
  import "./app.css";
  import "/public/css/main.css";
  import "./main.js";
  import * as d3 from "d3";

  let jugadoresData = [
    { src: "./images/nene24.svg", alturaCm: 124, color: '#FF6347' },
    { src: "./images/nene33.svg", alturaCm: 133, color: '#FF8C00' },
    { src: "./images/teen42.svg", alturaCm: 142, color: '#FFA500' },
    { src: "./images/elton54.svg", alturaCm: 154, color: '#008000' },
    { src: "./images/lorenzo63.svg", alturaCm: 163, color: '#0000FF' },
    { src: "./images/messi71.svg", alturaCm: 171, color: '#800080' },
    { src: "./images/locelso77.svg", alturaCm: 177, color: '#FF69B4' },
    { src: "./images/pezella87.svg", alturaCm: 187, color: '#00FFFF' },
    { src: "./images/edgar92.svg", alturaCm: 192, color: '#FF00FF' },
    { src: "./images/nickpope98.svg", alturaCm: 198, color: '#9400D3' }
  ];

  let scaleHeight = d3.scaleLinear().domain([124, 198]).range([190, 400]);
  let spacing = 120;

  let linesData = jugadoresData.map((jugador, index) => ({
    y: 600 - scaleHeight(jugador.alturaCm),
    x2: (index * spacing) + 60,
    color: jugador.color
  }));
</script>

<main id="container">
  <h1 class="title">Comparación de Alturas en el Fútbol</h1>
  <h3 class="subtitle">Diferencias de Altura en Jugadores</h3>
  
  <div class="grafico">
    <div class="flex-container">
      <svg width="1400" height="600">
        <g class="dashed-lines">
          {#each linesData as { y, x2, color }}
            <line x1="50" y1={y} x2={x2} y2={y} class="dashed-line" style="stroke: {color}; stroke-dasharray: 5,5; stroke-width: 2; opacity: 0.7;" />
          {/each}
        </g>
        <g class="jugadores">
          {#each jugadoresData as { src, alturaCm, color }, index}
            <image href={src} x={(index * spacing) + 50} y={600 - scaleHeight(alturaCm)} height={scaleHeight(alturaCm)} class="silueta" style="--color: {color};" />
            <text class="altura-text" x={(index * spacing) + 55} y={590 - scaleHeight(alturaCm)} fill={color} font-size="16" font-family="Arial, sans-serif">{alturaCm}cm</text>
          {/each}
        </g>
      </svg>
    </div>
  </div>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      display: flex;
      justify-content: center;
      align-items: flex-start;
      background-color: #233d4d;
      height: 100vh;
    }
    .title {
      text-align: center;
      font-family: "Poppins", sans-serif;
      color: #fe7f2d;
      padding: 1rem;
    }
    .subtitle {
      text-align: center;
      font-family: "Roboto", sans-serif;
      color: #ebebeb;
      padding-bottom: 1rem;
    }
    .grafico {
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .flex-container {
      display: flex;
      align-items: flex-end;
      
    }
    .dashed-line {
      stroke-width: 2;
      opacity: 0.7;
    }
    .silueta {
      filter: invert(100%) brightness(100%);
      transition: filter 0.3s ease-in-out;
    }
    .silueta:hover {
      filter: invert(100%) brightness(100%) drop-shadow(0 0 10px var(--color));
    }
    .silueta:hover + .altura-text {
      opacity: 1;
    }
    .altura-text {
      opacity: 0;
      transition: opacity 0.3s ease-in-out;
    }
  </style>
</main>
