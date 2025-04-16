<script>
  import "./app.css";
  import "/public/css/main.css";
  import "./main.js";
  import * as d3 from "d3";
  import { onMount } from 'svelte';

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

  // Nueva paleta de colores para los jugadores (moderna, más armoniosa)
  const actualizarColores = () => {
    const nuevaPaleta = [
      '#6366F1', // Indigo
      '#8B5CF6', // Violet
      '#EC4899', // Pink
      '#14B8A6', // Teal
      '#2DD4BF', // Turquoise
      '#F59E0B', // Amber
      '#10B981', // Emerald
      '#6366F1', // Indigo
      '#8B5CF6', // Violet
      '#F43F5E'  // Rose
    ];
    
    // Actualiza los colores pero mantiene las imágenes intactas
    jugadoresData = jugadoresData.map((jugador, index) => {
      return { ...jugador, color: nuevaPaleta[index] };
    });
    
    jugadoresData2 = jugadoresData2.map((jugador, index) => {
      return { ...jugador, color: nuevaPaleta[index] };
    });
  };

  // Combinar ambos grupos para la tabla
  let todosJugadores = [...jugadoresData, ...jugadoresData2];

  // Ajustar la escala de altura para que las siluetas sean más grandes
  let scaleHeight = d3.scaleLinear().domain([24, 100]).range([170, 350]);
  
  // Aumentar espaciado entre jugadores para evitar superposición completamente
  let spacing = 170; // Espacio entre jugadores
  
  let jugadorActivo = null; // jugador actualmente seleccionado por hover
  
  // Calcular el ancho total necesario para todos los jugadores
  let maxJugadores = Math.max(jugadoresData.length, jugadoresData2.length);
  
  // Ancho del SVG (ajustado para mejor centrado)
  let svgWidth = maxJugadores * spacing;
  
  // Asegurar un ancho mínimo
  svgWidth = Math.max(svgWidth, 1800);
  
  // Altura para el SVG 
  let svgHeight = 950;
  
  // Función para calcular el centrado de las siluetas
  function calcularOffsetX(numJugadores) {
    // Esta función calcula el desplazamiento necesario para centrar perfectamente
    // el grupo de jugadores en el SVG
    return (svgWidth - (numJugadores * spacing)) / 2;
  }
  
  // Ejecutar al cargar y al cambiar el tamaño de la ventana
  onMount(() => {
    actualizarColores(); // Actualizar colores al cargar
    
    // Función mejorada para aplicar estilo a los gráficos de Flourish
    const aplicarEstilosFlourish = () => {
      console.log("Aplicando estilos a gráficos Flourish...");
      
      // Seleccionar todos los iframes de Flourish
      const flourishFrames = document.querySelectorAll('.flourish-embed iframe');
      
      // Iterar por cada iframe
      flourishFrames.forEach((frame, index) => {
        try {
          // Intentar modificar el contenido del iframe
          if (frame && frame.contentWindow) {
            // Usar un evento de carga para asegurar que el iframe esté listo
            frame.addEventListener('load', function() {
              try {
                const styleEl = document.createElement('style');
                styleEl.textContent = `
                  body, .flourish-container, .flourish-chart, .flourish-chart div, .flourish-root-theme {
                    background-color: transparent !important;
                    color: #F8FAFC !important;
                  }
                  .flourish-chart text, .flourish-chart tspan {
                    fill: #F8FAFC !important;
                  }
                  .flourish-chart path, .flourish-chart line {
                    stroke: #6366F1 !important;
                  }
                  .flourish-chart .background-rect, .flourish-chart .background-shape {
                    fill: transparent !important;
                  }
                  .flourish-chart .axis-lines, .flourish-chart .tick line {
                    stroke: rgba(99, 102, 241, 0.5) !important;
                  }
                  .flourish-chart .legend text {
                    fill: #F8FAFC !important;
                  }
                `;
                
                // Intentar agregar el estilo al documento del iframe
                const iframeDoc = frame.contentDocument || frame.contentWindow.document;
                iframeDoc.head.appendChild(styleEl);
                
                console.log(`Estilos aplicados al iframe ${index}`);
              } catch (err) {
                console.error(`Error al aplicar estilos al iframe ${index}:`, err);
              }
            });
            
            // También aplicar estilos inmediatamente para iframes ya cargados
            try {
              const iframeDoc = frame.contentDocument || frame.contentWindow.document;
              if (iframeDoc && iframeDoc.head) {
                const styleEl = document.createElement('style');
                styleEl.textContent = `
                  body, .flourish-container, .flourish-chart, .flourish-chart div, .flourish-root-theme {
                    background-color: transparent !important;
                    color: #F8FAFC !important;
                  }
                  .flourish-chart text, .flourish-chart tspan {
                    fill: #F8FAFC !important;
                  }
                  .flourish-chart path, .flourish-chart line {
                    stroke: #6366F1 !important;
                  }
                  .flourish-chart .background-rect, .flourish-chart .background-shape {
                    fill: transparent !important;
                  }
                  .flourish-chart .axis-lines, .flourish-chart .tick line {
                    stroke: rgba(99, 102, 241, 0.5) !important;
                  }
                  .flourish-chart .legend text {
                    fill: #F8FAFC !important;
                  }
                `;
                iframeDoc.head.appendChild(styleEl);
              }
            } catch (e) {
              console.warn("No se pudo acceder al iframe ya cargado:", e);
            }
          }
        } catch (error) {
          console.error("Error al acceder al iframe:", error);
        }
      });
    };
    
    // Aplicar estilos a los gráficos después de un breve retraso
    // y también periódicamente para asegurar que se apliquen
    setTimeout(aplicarEstilosFlourish, 1000);
    setTimeout(aplicarEstilosFlourish, 2000);
    setTimeout(aplicarEstilosFlourish, 4000);
    
    // También aplicar cuando se cargan nuevos iframes
    const observador = new MutationObserver((mutations) => {
      mutations.forEach((mutation) => {
        if (mutation.addedNodes.length > 0) {
          setTimeout(aplicarEstilosFlourish, 1000);
        }
      });
    });
    
    observador.observe(document.body, { childList: true, subtree: true });
    
    return () => {
      observador.disconnect();
    };
  });
</script>

<header class="dashboard-header">
  <div class="header-container">
    <h1> Alturas en el Fútbol Profesional</h1>
    <p>Visualización comparativa entre diferentes jugadores según su posición en la cancha</p>
  </div>
</header>

<main id="container">
  <div class="grafico">
    <div class="visualization-container">
      <svg width="100%" height=500 style="overflow: visible" viewBox={`0 0 ${svgWidth} ${svgHeight}`} preserveAspectRatio="xMidYMid meet">
        <!-- Título del primer grupo -->
        <text 
          x={svgWidth / 2} 
          y="140"
          fill="#6366F1" 
          font-size="24" 
          font-family="Poppins, sans-serif"
          font-weight="bold"
          text-anchor="middle"
          class="equipo-titulo"
        >
        Newell's Old Boys 
        </text>
        
        <!-- Primer grupo de jugadores - Centrado horizontalmente -->
        <g class="jugadores" transform={`translate(${calcularOffsetX(jugadoresData.length)}, 30)`}> 
          {#each jugadoresData as jugador, index}
            <g 
              class="jugador-container"
              role="group"
              on:mouseenter={() => jugadorActivo = jugador}
              on:mouseleave={() => jugadorActivo = null}
            >
              <line 
                x1={(index * spacing)} 
                y1={400 - scaleHeight(jugador.alturaReal)} 
                x2={(index * spacing) + 60} 
                y2={400 - scaleHeight(jugador.alturaReal)} 
                class="dashed-line" 
                style="stroke: {jugador.color}; stroke-dasharray: 5,5; stroke-width: 2;" 
              />
              <image 
                href={jugador.src} 
                x={(index * spacing) + 30} 
                y={400 - scaleHeight(jugador.alturaReal)} 
                height={scaleHeight(jugador.alturaReal)}
                class="silueta" 
                style="--color: {jugador.color};" 
              />
              <text 
                class="altura-text" 
                x={(index * spacing) + 60} 
                y={390 - scaleHeight(jugador.alturaReal)} 
                fill={jugador.color} 
                font-size="16" 
                font-family="Poppins, sans-serif"
                font-weight="bold"
              >
                {(jugador.alturaReal + 100) / 100} m
              </text>
            </g>
          {/each}
        </g>
        
        <!-- Título del segundo grupo -->
        <text 
          x={svgWidth / 2} 
          y="570" 
          fill="#F59E0B" 
          font-size="24" 
          font-family="Poppins, sans-serif"
          font-weight="bold"
          text-anchor="middle"
          class="equipo-titulo"
        >
        Rosario Central
        </text>
        
        <!-- Segundo grupo de jugadores - Centrado horizontalmente -->
        <g class="jugadores-grupo2" transform={`translate(${calcularOffsetX(jugadoresData2.length)}, 460)`}> 
          {#each jugadoresData2 as jugador, index}
            <g 
              class="jugador-container"
              role="group"
              on:mouseenter={() => jugadorActivo = jugador}
              on:mouseleave={() => jugadorActivo = null}
            >
              <line 
                x1={(index * spacing)} 
                y1={400 - scaleHeight(jugador.alturaReal)} 
                x2={(index * spacing) + 60} 
                y2={400 - scaleHeight(jugador.alturaReal)} 
                class="dashed-line" 
                style="stroke: {jugador.color}; stroke-dasharray: 5,5; stroke-width: 2;" 
              />
              <image 
                href={jugador.src} 
                x={(index * spacing) + 30} 
                y={400 - scaleHeight(jugador.alturaReal)} 
                height={scaleHeight(jugador.alturaReal)}
                class="silueta" 
                style="--color: {jugador.color};" 
              />
              <text 
                class="altura-text" 
                x={(index * spacing) + 60} 
                y={390 - scaleHeight(jugador.alturaReal)} 
                fill={jugador.color} 
                font-size="16" 
                font-family="Poppins, sans-serif"
                font-weight="bold"
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
      Esta visualización compara la altura de diferentes tipos de jugadores de fútbol, desde niños hasta profesionales que ocupan diversas posiciones dentro del campo. Se puede observar que los defensores y arqueros tienden a ser más altos, lo que les otorga ciertas ventajas físicas en sus roles. En cambio, los delanteros y mediocampistas presentan una mayor variedad de estaturas. Esto refleja que cada rol en el campo tiene exigencias físicas distintas, y demuestra que el talento y el rendimiento pueden encontrarse en jugadores de cualquier altura. La adición de un segundo grupo de comparación permite analizar estas tendencias con una muestra más amplia y observar similitudes y diferencias entre distintos equipos o ligas. Esto proporciona una perspectiva más enriquecedora sobre la diversidad física presente en el fútbol moderno.
    </p>
  </div>

  <!-- Nueva sección: Cuadros comparativos separados -->
  <div class="tablas-container">
    <!-- Cuadro Comparativo Newell's Old Boys -->
    <div class="cuadro-comparativo cuadro-newells">
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
    <div class="cuadro-comparativo cuadro-central">
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
        data-background-color="transparent"
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
  <div class="chart-section chart-section-alt">
    <h2 class="chart-title">Comparación de Altura por Posición entre Equipos</h2>
    <p class="chart-description">
      Este gráfico de área apilado muestra la altura promedio de los jugadores según su posición en el campo para dos equipos diferentes. La visualización permite comparar cómo varía la estatura entre defensores, mediocampistas y delanteros en cada equipo, destacando tendencias estratégicas o diferencias en perfiles físicos.

    </p>
    <div class="chart-wrapper">
      <div
        class="flourish-embed flourish-chart"
        data-src="visualisation/22572094"
        data-background-color="transparent"
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
        data-background-color="transparent"
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
      <p>Proyecto de Visualización de Datos en el Fútbol</p>
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
      background-color: #0F172A; /* Azul oscuro moderno para el fondo */
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      font-family: "Poppins", "DM Sans", sans-serif;
      color: #F8FAFC;
      background-image: linear-gradient(to bottom, #0F172A, #1E293B);
    }

    #container {
      width: 100%;
      max-width: 1400px; /* Aumentado para mejor visualización en pantallas grandes */
      margin: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 0 25px;
    }

    .grafico {
      width: 100%;
      overflow-x: auto;
      overflow-y: visible;
      margin: 2rem 0; /* Reducido de 2.5rem a 2rem */
      padding-bottom: 30px; /* Reducido de 40px a 30px */
      scrollbar-width: thin;
      scrollbar-color: #6366F1 #1E293B; /* Actualizado a un tono indigo */
      -webkit-overflow-scrolling: touch;
      position: relative;
      border-radius: 12px;
      background-color: rgba(30, 41, 59, 0.4);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      padding: 20px;
    }

    .grafico::-webkit-scrollbar {
      height: 10px;
    }

    .grafico::-webkit-scrollbar-track {
      background: #1E293B;
      border-radius: 5px;
    }

    .grafico::-webkit-scrollbar-thumb {
      background-color: #6366F1; /* Actualizado a indigo */
      border-radius: 5px;
    }

    .visualization-container {
      min-width: 100%;
      padding: 10px 0; /* Reducido de 20px a 10px */
    }

    .dashed-line {
      opacity: 0;
      transition: opacity 0.3s ease-in-out;
      pointer-events: none;
    }

    .jugador-container:hover .dashed-line {
      opacity: 0.8;
    }

    .silueta {
      filter: invert(100%) brightness(100%);
      transition: filter 0.3s ease-in-out, transform 0.2s ease-out;
      width: auto;
      max-width: 100px; /* Aumentado para mejor visualización */
      object-fit: contain;
      transform-origin: bottom center;
    }

    .jugador-container:hover .silueta {
      filter: invert(100%) brightness(100%) drop-shadow(0 0 12px var(--color));
      z-index: 10;
    }

    .altura-text {
      opacity: 0;
      transition: opacity 0.3s ease-in-out;
      font-weight: bold;
      text-anchor: middle;
      filter: drop-shadow(0 0 2px rgba(0, 0, 0, 0.8));
    }

    .jugador-container:hover .altura-text {
      opacity: 1;
    }

    .equipo-titulo {
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.4);
      filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.1));
    }

    .tooltip {
      position: fixed;
      bottom: 40px;
      right: 40px;
      background-color: #1E293B;
      color: #F8FAFC;
      padding: 1.4rem 2rem;
      border-radius: 14px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3), 0 0 15px rgba(99, 102, 241, 0.5); /* Actualizado a indigo */
      font-family: 'Poppins', sans-serif;
      transition: all 0.3s ease;
      z-index: 100;
      border-left: 4px solid #6366F1; /* Actualizado a indigo */
      backdrop-filter: blur(10px);
      min-width: 220px;
      transform: translateY(0);
      animation: tooltip-appear 0.3s ease-out;
    }

    @keyframes tooltip-appear {
      0% { opacity: 0; transform: translateY(10px); }
      100% { opacity: 1; transform: translateY(0); }
    }

    .tooltip h4 {
      margin: 0;
      margin-bottom: 0.7rem;
      font-size: 1.3rem;
      color: #6366F1; /* Actualizado a indigo */
      border-bottom: 1px solid rgba(99, 102, 241, 0.3); /* Actualizado a indigo */
      padding-bottom: 0.5rem;
    }

    .tooltip p {
      margin: 0.4rem 0;
      font-size: 1.05rem;
    }

    .narrativa {
      max-width: 1000px;
      margin: 2.5 rem auto; 
      padding: 2.5rem;
      color: #F8FAFC;
      font-family: 'Poppins', sans-serif;
      line-height: 2;
      text-align: justify;
      columns: 2;
      column-gap:  3rem !important; 
      background-color: rgba(30, 41, 59, 0.4);
      border-radius: 12px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
    }

    .narrativa {
        padding: 1.5rem;
      }

      .narrativa p {
      margin: 0;
      margin-bottom: 1.4rem;
    }


    
    /* Media query para narrativa en pantallas pequeñas */
    @media (max-width: 768px) {
      .narrativa {
        columns: 1;
        padding: 1.5rem;
      }
    }

    .dashboard-header {
      width: 100%;
      padding: 2.5rem 0;
      text-align: center;
      background: linear-gradient(135deg, rgba(99, 102, 241, 0.2), rgba(79, 70, 229, 0.1));
      margin-bottom: 2rem;
      border-radius: 0 0 16px 16px;
      box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    }

    .header-container {
      position: relative;
      z-index: 1;
      max-width: 1000px;
      margin: 0 auto;
    }

    .header-container h1 {
      font-size: 2.8rem;
      margin-bottom: 0.5rem;
      background: linear-gradient(to right, #6366F1, #F59E0B); /* Actualizado a indigo y naranja */
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      text-fill-color: transparent;
      display: inline-block;
      font-weight: 700;
      letter-spacing: -0.5px;
    }

    .header-container p {
      font-size: 1.2rem;
      opacity: 0.85;
      max-width: 700px;
      margin: 0 auto;
    }


    .tablas-container {
      display: flex;
      justify-content: space-between;
      gap: 2rem;
      width: 100%;
      max-width: 1200px;
      margin: 2rem auto;
      flex-wrap: wrap;
    }

    .cuadro-comparativo {
      flex: 1 1 45%;
      min-width: 300px;
      padding: 1.5rem;
      border-radius: 12px;
      background-color: rgba(30, 41, 59, 0.4);
      box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    }

    .cuadro-newells {
      border-left: 4px solid #6366F1;
    }

    .cuadro-central {
      border-left: 4px solid #F59E0B;
    }

    .cuadro-comparativo h3 {
      margin-top: 0;
      margin-bottom: 1.5rem;
      font-size: 1.4rem;
      color: #F8FAFC;
      text-align: center;
      padding-bottom: 0.8rem;
      border-bottom: 1px solid rgba(203, 213, 225, 0.2);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
      font-size: 0.95rem;
    }

    th {
      text-align: left;
      padding: 0.8rem 1rem;
      border-bottom: 2px solid rgba(99, 102, 241, 0.3);
      color: #CBD5E1;
      font-weight: 600;
    }

    td {
      padding: 0.8rem 1rem;
      border-bottom: 1px solid rgba(203, 213, 225, 0.1);
      color: #F8FAFC;
    }

    tr:last-child td {
      border-bottom: none;
    }

    tr:hover td {
      background-color: rgba(99, 102, 241, 0.1);
    }

    .chart-section {
      width: 100%;
      max-width: 1200px;
      margin: 3rem auto;
      padding: 2rem;
      border-radius: 12px;
      background-color: rgba(30, 41, 59, 0.4);
      box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
      border-left: 4px solid #6366F1;
    }

    .chart-section-alt {
      border-left: 4px solid #F59E0B;
    }

    .chart-title {
      font-size: 1.8rem;
      color: #F8FAFC;
      margin-bottom: 1rem;
      text-align: center;
    }


    .chart-description {
      font-size: 1.05rem;
      line-height: 1.6;
      color: #E2E8F0;
      margin-bottom: 2rem;
      text-align: center;
      max-width: 900px;
      margin-left: auto;
      margin-right: auto;
    }

    .chart-wrapper {
      width: 100%;
      min-height: 400px;
      background-color: transparent; /* Fondo transparente para todos los gráficos */
      border-radius: 8px;
      overflow: hidden;
    }

    /* Estilo específico para los div del Flourish */
    .flourish-embed {
    width: 50%;
    height: 550px;
    margin: 0 auto;
    display: block;
    background-color: transparent !important;
  }


    /* Asegurarse de que los iframes de Flourish también tengan fondo transparente */
    .flourish-embed iframe {
      background-color: transparent !important;
    }

    .footer {
      width: 100%;
      margin-top: 4rem;
      padding: 2rem 0;
      background-color: rgba(15, 23, 42, 0.8);
      border-top: 1px solid rgba(99, 102, 241, 0.2);
    }

    .footer-content {
      width: 100%;
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 2rem;
      text-align: center;
      color: #CBD5E1;
    }

    .footer p {
      margin: 0.5rem 0;
    }

    .footer ul {
      list-style: none;
      padding: 0;
      margin: 1rem 0;
      display: flex;
      justify-content: center;
      gap: 2rem;
      flex-wrap: wrap;
    }

    .footer a {
      color: #6366F1;
      text-decoration: none;
      transition: color 0.2s ease;
    }

    .footer a:hover {
      color: #818CF8;
      text-decoration: underline;
    }

    @media (max-width: 768px) {
      .tablas-container {
        flex-direction: column;
      }
      
      .cuadro-comparativo {
        margin-bottom: 1.5rem;
      }
      
      .footer ul {
        flex-direction: column;
        gap: 0.8rem;
      }
      
      h1 {
        font-size: 2rem;
      }
      
      .chart-section {
        padding: 1.5rem;
      }
    }
  </style>
</main>