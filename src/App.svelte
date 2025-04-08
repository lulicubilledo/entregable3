<script>
  import "./app.css";
  import "/public/css/main.css";
  import "./main.js";
  import * as d3 from "d3";

  let jugadoresData = [
    { src: "./images/nene24.svg", alturaReal: 24, color: '#FF6347', posicion: '-', nombre: 'Tomás' },
    { src: "./images/nene33.svg", alturaReal: 33, color: '#FF8C00', posicion: '-', nombre: 'Lucas' },
    { src: "./images/teen42.svg", alturaReal: 42, color: '#FF0000', posicion: '-', nombre: 'Mateo' },
    { src: "./images/elton54.svg", alturaReal: 54, color: '#008000', posicion: 'Mediocampista', nombre: 'Élton José Xavier Gomes' },
    { src: "./images/lorenzo63.svg", alturaReal: 63, color: '#0000FF', posicion: 'Delantero', nombre: 'Lorenzo Insigne' },
    { src: "./images/messi71.svg", alturaReal: 71, color: '#800080', posicion: 'Delantero', nombre: 'Lionel Andrés Messi' },
    { src: "./images/locelso77.svg", alturaReal: 77, color: '#FF69B4', posicion: 'Mediocampista', nombre: 'Giovani Lo Celso' },
    { src: "./images/pezella87.svg", alturaReal: 87, color: '#00FFFF', posicion: 'Defensor', nombre: 'Germán Pezzella' },
    { src: "./images/edgar92.svg", alturaReal: 92, color: '#32CD32', posicion: 'Defensor', nombre: 'Edgar González' },
    { src: "./images/nickpope98.svg", alturaReal: 98, color: '#9400D3', posicion: 'Defensor', nombre: 'Harry Souttar' }
  ];

  let scaleHeight = d3.scaleLinear().domain([24, 98]).range([150, 320]);
  let spacing = 120;

  let jugadorActivo = null; // jugador actualmente seleccionado por hover
</script>

<main id="container">
  <h1 class="title">Comparación de Alturas en el Fútbol</h1>
  <h3 class="subtitle">Diferencias de Altura en Jugadores por Posición</h3>
  
  <div class="grafico">
    <div class="flex-container">
      <svg width="1400" height="650">
        <g class="jugadores" transform="translate(50, 50)">
          {#each jugadoresData as jugador, index}
              <g 
              class="jugador-container"
              role="group"
              on:mouseenter={() => jugadorActivo = jugador}
              on:mouseleave={() => jugadorActivo = null}
            >
        
              <line 
                x1="10" 
                y1={600 - scaleHeight(jugador.alturaReal)} 
                x2={(index * spacing) + 60} 
                y2={600 - scaleHeight(jugador.alturaReal)} 
                class="dashed-line" 
                style="stroke: {jugador.color}; stroke-dasharray: 5,5; stroke-width: 2;" 
              />
              <image 
                href={jugador.src} 
                x={(index * spacing) + 50} 
                y={600 - scaleHeight(jugador.alturaReal)} 
                height={scaleHeight(jugador.alturaReal)} 
                class="silueta" 
                style="--color: {jugador.color};" 
              />
              <text 
                class="altura-text" 
                x={(index * spacing) + 55} 
                y={590 - scaleHeight(jugador.alturaReal)} 
                fill={jugador.color} 
                font-size="16" 
                font-family="Arial, sans-serif"
              >
                {(jugador.alturaReal + 100) / 100} m
              </text>
            </g>
          {/each}
        </g>
      </svg>
    </div>
  </div>

  <!-- Tooltip flotante -->
  {#if jugadorActivo}
    <div class="tooltip">
      <h4>{jugadorActivo.nombre}</h4>
      <p>Altura: {((jugadorActivo.alturaReal + 100) / 100).toFixed(2)} m</p>
      <p>Posición: {jugadorActivo.posicion !== '-' ? jugadorActivo.posicion : 'Sin posición'}</p>
    </div>
  {/if}

  <div class="narrativa">
    <p>
      Esta visualización compara la altura de diferentes tipos de jugadores de fútbol, desde niños hasta profesionales que ocupan diversas posiciones dentro del campo. Se puede observar que los defensores y arqueros tienden a ser más altos, lo que les otorga ciertas ventajas físicas en sus roles. En cambio, los delanteros y mediocampistas presentan una mayor variedad de estaturas. Esto refleja que cada rol en el campo tiene exigencias físicas distintas, y demuestra que el talento y el rendimiento pueden encontrarse en jugadores de cualquier altura.
    </p>
  </div>

  <div class="cuadro-comparativo">
    <h3>Cuadro Comparativo</h3>
    <table>
      <thead>
        <tr>
          <th>Jugador</th>
          <th>Altura</th>
          <th>Posición</th>
        </tr>
      </thead>
      <tbody>
        {#each jugadoresData as { nombre, alturaReal, posicion }}
          <tr>
            <td>{nombre}</td>
            <td>{((alturaReal + 100) / 100).toFixed(2)} m</td>
            <td>{posicion}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      margin: 0;
      background-color: #233d4d;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: flex-start;
    }

    #container {
      width: 100%;
      max-width: 1400px;
      margin-top: 1rem;
      display: flex;
      flex-direction: column;
      align-items: center;
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
      margin-top: -250px;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .flex-container {
      display: flex;
      align-items: flex-end;
    }

    .dashed-line {
      opacity: 0;
      transition: opacity 0.3s ease-in-out;
      pointer-events: none;
    }

    .jugador-container:hover .dashed-line {
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

    .tooltip {
      position: fixed;
      bottom: 40px;
      right: 40px;
      background-color: #1e2f3d;
      color: #f1f1f1;
      padding: 1rem 1.5rem;
      border-radius: 12px;
      box-shadow: 0 0 12px rgba(255, 255, 255, 0.1);
      font-family: 'Roboto', sans-serif;
      transition: opacity 0.3s ease-in-out;
      z-index: 10;
    }

    .tooltip h4 {
      margin: 0;
      margin-bottom: 0.5rem;
      font-size: 1.2rem;
      color: #fe7f2d;
    }

    .narrativa {
      max-width: 800px;
      margin: 1rem auto;
      padding: 1rem;
      color: #f1f1f1;
      font-family: 'Roboto', sans-serif;
      line-height: 1.6;
      text-align: justify;
      columns: 2;
      column-gap: 2rem;
      margin-top: 80px;
    }

    .narrativa p {
      margin: 0;
    }

    .cuadro-comparativo {
      max-width: 800px;
      margin: 2rem auto;
      padding: 1rem;
      background-color: transparent;
      border-radius: 12px;
      color: #f1f1f1;
      font-family: 'Roboto', sans-serif;
    }

    .cuadro-comparativo h3 {
      text-align: center;
      margin-bottom: 1rem;
      color: #fe7f2d;
    }

    .cuadro-comparativo table {
      width: 100%;
      border-collapse: collapse;
    }

    .cuadro-comparativo th,
    .cuadro-comparativo td {
      border: 1px solid #445f73;
      padding: 0.5rem;
      text-align: center;
    }

    .cuadro-comparativo th {
      background-color: #1e2f3d;
    }

    svg {
      display: block;
      margin: 0 auto;
    }

    p {
      margin-top: 10px
    }
  </style>
</main>