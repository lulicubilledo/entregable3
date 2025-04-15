<script>
  import "./app.css";
  import "/public/css/main.css";
  import "./main.js";
  import * as d3 from "d3";

  // Primer grupo de jugadores
  let jugadoresData = [
    { src: "./images/messi71.svg", alturaReal: 24, color: '#FF6347', posicion: 'Extremo Izquierdo', nombre: 'Lionel Andrés Messi' },
    { src: "./images/maxi.svg", alturaReal: 33, color: '#FF8C00', posicion: 'Extremo Derecho', nombre: 'Maximiliano Rodríguez' },
    { src: "./images/alejotabares.svg", alturaReal: 42, color: '#FF0000', posicion: 'Lateral Derecho', nombre: 'Marcelo Bielsa' },
    { src: "./images/miltoncasco.svg", alturaReal: 54, color: '#008000', posicion: 'Lateral Izquierdo', nombre: 'Milton Casco' },
    { src: "./images/keylor.svg", alturaReal: 63, color: '#0000FF', posicion: 'Arquero', nombre: 'Keylor Navas' },
    { src: "./images/heinze.svg", alturaReal: 71, color: '#800080', posicion: 'Defensor Central', nombre: 'Gabriel Heinze' },
    { src: "./images/maradona.svg", alturaReal: 77, color: '#FF69B4', posicion: 'Mediocampista Central', nombre: 'Diego Armando Maradona' },
    { src: "./images/scocco.svg", alturaReal: 87, color: '#00FFFF', posicion: 'Delantero', nombre: 'Ignacio Scocco' },
    { src: "./images/pabloperez.svg", alturaReal: 92, color: '#32CD32', posicion: 'Mediocampista Derecho', nombre: 'Pablo Perez' },
    { src: "./images/sensini.svg", alturaReal: 98, color: '#9400D3', posicion: 'Mediocampista Izquierdo', nombre: 'Américo Gallego' }
  ];

  // Segundo grupo de jugadores
  let jugadoresData2 = [
    { src: "./images/danilo_gerlo.svg", alturaReal: 23, color: '#FF6347', posicion: 'Extremo Izquierdo', nombre: 'Santiago López' },
    { src: "./images/dimaria.svg", alturaReal: 35, color: '#FF8C00', posicion: 'Extremo Derecho', nombre: 'Ángel Di Maria' },
    { src: "./images/hernan_diaz.svg", alturaReal: 45, color: '#FF0000', posicion: 'Lateral Derecho', nombre: 'Hernán Díaz' },
    { src: "./images/lautaroblanco.svg", alturaReal: 56, color: '#008000', posicion: 'Lateral Izquierdo', nombre: 'Lautaro Blanco' },
    { src: "./images/jorgebroun.svg", alturaReal: 50, color: '#0000FF', posicion: 'Arquero', nombre: 'Jorge Broun' },
    { src: "./images/edgardo_bauza.svg", alturaReal: 43, color: '#800080', posicion: 'Defensor Central', nombre: 'Edgardo Bauza' },
    { src: "./images/locelso77.svg", alturaReal: 38, color: '#FF69B4', posicion: 'Mediocampista Central', nombre: 'Giovani Lo Celso' },
    { src: "./images/ruben.svg", alturaReal: 32, color: '#00FFFF', posicion: 'Delantero', nombre: 'Marcos Ruben' },
    { src: "./images/coudet.svg", alturaReal: 28, color: '#32CD32', posicion: 'Mediocampista Derecho', nombre: 'Eduardo Coudet' },
    { src: "./images/gino_infantino.svg", alturaReal: 25, color: '#9400D3', posicion: 'Mediocampista Izquierdo', nombre: 'Gino Infantino' }
  ];

  // Combinar ambos grupos para la tabla
  let todosJugadores = [...jugadoresData, ...jugadoresData2];

  let scaleHeight = d3.scaleLinear().domain([24, 100]).range([150, 320]);
  let spacing = 120;

  let jugadorActivo = null; // jugador actualmente seleccionado por hover
</script>

<header class="dashboard-header">
  <h1>⚽ Alturas en el Fútbol Profesional</h1>
  <p>Visualización comparativa entre diferentes jugadores según su posición en la cancha</p>
</header>

<main id="container">
  <div class="grafico">
    <div class="flex-container">
      <svg width="1400" height="1300">
        <!-- Título del primer grupo -->
        <text 
          x="80" 
          y="250" 
          fill="#fe7f2d" 
          font-size="18" 
          font-family="Poppins, sans-serif"
          font-weight="bold"
        >
        Newell's Old Boys 
        </text>
        
        <!-- Primer grupo de jugadores -->
        <g class="jugadores" transform="translate(50, 60)">
          {#each jugadoresData as jugador, index}
              <g 
              class="jugador-container"
              role="group"
              on:mouseenter={() => jugadorActivo = jugador}
              on:mouseleave={() => jugadorActivo = null}
            >
        
              <line 
                x1="10" 
                y1={500 - scaleHeight(jugador.alturaReal)} 
                x2={(index * spacing) + 60} 
                y2={500 - scaleHeight(jugador.alturaReal)} 
                class="dashed-line" 
                style="stroke: {jugador.color}; stroke-dasharray: 5,5; stroke-width: 2;" 
              />
              <image 
                href={jugador.src} 
                x={(index * spacing) + 50} 
                y={500 - scaleHeight(jugador.alturaReal)} 
                height={scaleHeight(jugador.alturaReal)} 
                class="silueta" 
                style="--color: {jugador.color};" 
              />
              <text 
                class="altura-text" 
                x={(index * spacing) + 55} 
                y={490 - scaleHeight(jugador.alturaReal)} 
                fill={jugador.color} 
                font-size="16" 
                font-family="Arial, sans-serif"
              >
                {(jugador.alturaReal + 100) / 100} m
              </text>
            </g>
          {/each}
        </g>
        
        <!-- Título del segundo grupo -->
        <text 
          x="80" 
          y="750" 
          fill="#fe7f2d" 
          font-size="18" 
          font-family="Poppins, sans-serif"
          font-weight="bold"
        >
        Rosario Central
        </text>
        
        <!-- Segundo grupo de jugadores -->
        <g class="jugadores-grupo2" transform="translate(50, 630)">
          {#each jugadoresData2 as jugador, index}
              <g 
              class="jugador-container"
              role="group"
              on:mouseenter={() => jugadorActivo = jugador}
              on:mouseleave={() => jugadorActivo = null}
            >
        
              <line 
                x1="10" 
                y1={500 - scaleHeight(jugador.alturaReal)} 
                x2={(index * spacing) + 60} 
                y2={500 - scaleHeight(jugador.alturaReal)} 
                class="dashed-line" 
                style="stroke: {jugador.color}; stroke-dasharray: 5,5; stroke-width: 2;" 
              />
              <image 
                href={jugador.src} 
                x={(index * spacing) + 50} 
                y={500 - scaleHeight(jugador.alturaReal)} 
                height={scaleHeight(jugador.alturaReal)} 
                class="silueta" 
                style="--color: {jugador.color};" 
              />
              <text 
                class="altura-text" 
                x={(index * spacing) + 55} 
                y={490 - scaleHeight(jugador.alturaReal)} 
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
    <p>
      La adición de un segundo grupo de comparación permite analizar estas tendencias con una muestra más amplia y observar similitudes y diferencias entre distintos equipos o ligas. Esto proporciona una perspectiva más enriquecedora sobre la diversidad física presente en el fútbol moderno.
    </p>
  </div>

  <!-- Nueva sección: Cuadros comparativos separados -->
  <div class="tablas-container">
    <!-- Cuadro Comparativo Newell's Old Boys -->
    <div class="cuadro-comparativo">
      <h3>Jugadores de Newell's Old Boys</h3>
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

    <!-- Cuadro Comparativo Rosario Central -->
    <div class="cuadro-comparativo">
      <h3>Jugadores de Rosario Central</h3>
      <table>
        <thead>
          <tr>
            <th>Jugador</th>
            <th>Altura</th>
            <th>Posición</th>
          </tr>
        </thead>
        <tbody>
          {#each jugadoresData2 as { nombre, alturaReal, posicion }}
            <tr>
              <td>{nombre}</td>
              <td>{((alturaReal + 100) / 100).toFixed(2)} m</td>
              <td>{posicion}</td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>

  <!-- Flourish Embed 1 con título y descripción -->
  <div class="chart-section">
    <h2 class="chart-title">Distribución de Alturas por Posición</h2>
    <p class="chart-description">
      Este gráfico muestra la distribución de alturas de los jugadores según su posición en el campo. Los arqueros y defensores centrales suelen tener mayor estatura, mientras que los extremos y mediocampistas suelen ser más bajos. Esto refleja las demandas físicas específicas de cada posición.
    </p>
    <div class="chart-wrapper">
      <div
        class="flourish-embed flourish-chart"
        data-src="visualisation/22564630"
      >
        <script src="https://public.flourish.studio/resources/embed.js"></script>
        <noscript>
          <img
            src="https://public.flourish.studio/visualisation/22564630/thumbnail"
            width="100%"
            alt="Gráfico de distribución de alturas por posición"
          />
        </noscript>
      </div>
    </div>
  </div>

  <!-- Flourish Embed 2 con título y descripción -->
  <div class="chart-section">
    <h2 class="chart-title">Evolución de la Altura Promedio en el Fútbol (1980-2025)</h2>
    <p class="chart-description">
      A lo largo de las últimas décadas, la altura promedio de los jugadores profesionales ha experimentado un incremento gradual. Este gráfico muestra esta evolución y destaca cómo los requerimientos físicos en el fútbol moderno han cambiado, con una tendencia hacia jugadores de mayor estatura, especialmente en posiciones defensivas.
    </p>
    <div class="chart-wrapper">
      <div
        class="flourish-embed flourish-chart"
        data-src="visualisation/22572094"
      >
        <script src="https://public.flourish.studio/resources/embed.js"></script>
        <noscript>
          <img
            src="https://public.flourish.studio/visualisation/22572094/thumbnail"
            width="100%"
            alt="Gráfico de evolución de altura promedio en el fútbol"
          />
        </noscript>
      </div>
    </div>
  </div>

  <!-- Flourish Embed 3 con título y descripción -->
  <div class="chart-section">
    <h2 class="chart-title">Comparativa de Alturas entre Ligas</h2>
    <p class="chart-description">
      Las diferentes ligas de fútbol alrededor del mundo presentan variaciones en la altura promedio de sus jugadores. Este gráfico compara las estaturas promedio entre las principales ligas, revelando diferencias regionales y estilísticas en el juego. Las ligas del norte de Europa tienden a contar con jugadores de mayor estatura en comparación con las ligas sudamericanas, donde se privilegia más la técnica y la agilidad.
    </p>
    <div class="chart-wrapper">
      <div
        class="flourish-embed flourish-chart"
        data-src="visualisation/22572117"
      >
        <script src="https://public.flourish.studio/resources/embed.js"></script>
        <noscript>
          <img
            src="https://public.flourish.studio/visualisation/22572117/thumbnail"
            width="100%"
            alt="Gráfico comparativo de alturas entre ligas"
          />
        </noscript>
      </div>
    </div>
  </div>

  <footer class="footer">
    <div class="footer-content">
      <p>Proyecto de Visualización de Datos</p>
      <p>Integrantes:</p>
      <ul>
        <li><a href="https://www.linkedin.com/in/tu-nombre" target="_blank">Josefina Casas Pardo</a></li>
        <li><a href="https://github.com/lulicubilledo" target="_blank">Luisina Cubilledo</a></li>
      </ul>
      <p>&copy; 2025 · Universidad Torcuato Di Tella </p>
    </div>
  </footer>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      margin: 0;
      background-color: #233d4d;
      min-height: 100vh;
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

    .grafico {
      margin-top: -100px;
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
      margin-bottom: 1rem;
    }

    /* Estilos para las tablas separadas */
    .tablas-container {
      display: flex;
      justify-content: space-between;
      flex-wrap: wrap;
      max-width: 1100px;
      width: 100%;
      margin: 2rem auto;
    }

    .cuadro-comparativo {
      width: 48%;
      min-width: 350px;
      margin: 1rem auto;
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

    /* Estilos para las secciones de gráficos */
    .chart-section {
      width: 100%;
      max-width: 1100px;
      margin: 4rem auto;
    }

    .chart-title {
      color: #fe7f2d;
      font-size: 1.8rem;
      text-align: center;
      margin-bottom: 1rem;
      font-family: "DM Sans", sans-serif;
    }

    .chart-description {
      color: #f1f1f1;
      font-family: "Roboto", sans-serif;
      line-height: 1.6;
      text-align: justify;
      max-width: 900px;
      margin: 0 auto 2rem auto;
      padding: 0 1rem;
    }

    .chart-wrapper {
      max-width: 1100px;
      margin: auto;
      margin-bottom: 50px;
      aspect-ratio: 16 / 9;
      height: auto;
    }

    /* Estilos existentes */
    @import url("https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,100..1000;1,9..40,100..1000&display=swap");

    * {
      font-family: "DM Sans", sans-serif;
    }

    .header {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      margin-top: 50px;
      margin-bottom: 60px;
    }

    .headline {
      font-size: 30px;
      line-height: 1.2;
      font-weight: normal;
      text-align: center;
      margin: 20px;
    }

    .headline b {
      display: block;
      font-size: 35px;
      margin-bottom: 5px;
    }

    .footer {
      background-color: #14213d;
      padding: 2rem 1rem;
      color: #f1f1f1;
      text-align: center;
      font-family: "Roboto", sans-serif;
      margin-top: 4rem;
      width: 100%;
    }

    .footer a {
      color: #fe7f2d;
      text-decoration: none;
      margin: 0 5px;
      transition: color 0.2s ease;
    }

    .footer a:hover {
      color: #ffffff;
    }

    .footer-content ul {
      list-style: none;
      padding: 0;
      margin: 0.5rem 0 1rem 0;
    }

    .footer-content li {
      display: inline;
      margin: 0 10px;
    }

    .dashboard-header {
      width: 100%;
      background-color: #14213d;
      color: #fe7f2d;
      padding: 1.5rem;
      text-align: center;
      box-shadow: 0 2px 5px rgba(0,0,0,0.3);
    }

    /* Media queries para dispositivos móviles */
    @media (max-width: 768px) {
      .tablas-container {
        flex-direction: column;
      }
      
      .cuadro-comparativo {
        width: 95%;
      }
      
      .narrativa {
        columns: 1;
      }
    }
  </style>
</main>