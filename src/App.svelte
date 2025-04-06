<script>
  import "./app.css";
  import "/public/css/main.css";
  import "./main.js";
  import * as d3 from "d3";

  let jugadoresData = [
  { src: "./images/nene24.svg", alturaCm: 124, color: '#FF6347', posicion: '-', nombre: 'Tomás' },
  { src: "./images/nene33.svg", alturaCm: 133, color: '#FF8C00', posicion: '-', nombre: 'Lucas' },
  { src: "./images/teen42.svg", alturaCm: 142, color: '#FFA500', posicion: '-', nombre: 'Mateo' },
  { src: "./images/elton54.svg", alturaCm: 154, color: '#008000', posicion: 'Mediocampista', nombre: 'Élton José Xavier Gomes' },
  { src: "./images/lorenzo63.svg", alturaCm: 163, color: '#0000FF', posicion: 'Delantero', nombre: 'Lorenzo Insigne' },
  { src: "./images/messi71.svg", alturaCm: 171, color: '#800080', posicion: 'Delantero', nombre: 'Lionel Andrés Messi' },
  { src: "./images/locelso77.svg", alturaCm: 177, color: '#FF69B4', posicion: 'Mediocampista', nombre: 'Giovani Lo Celso' },
  { src: "./images/pezella87.svg", alturaCm: 187, color: '#00FFFF', posicion: 'Defensor', nombre: 'Germán Pezzella' },
  { src: "./images/edgar92.svg", alturaCm: 192, color: '#FF00FF', posicion: 'Defensor', nombre: 'Edgar González' },
  { src: "./images/nickpope98.svg", alturaCm: 198, color: '#9400D3', posicion: 'Defensor', nombre: 'Harry Souttar' }
];

  let scaleHeight = d3.scaleLinear().domain([124, 198]).range([150, 320]);
  let spacing = 120;

</script>

<main id="container">
  <h1 class="title">Comparación de Alturas en el Fútbol</h1>
  <h3 class="subtitle">Diferencias de Altura en Jugadores por Posición</h3>
  
  <div class="grafico">
    <div class="flex-container">
      <svg width="1400" height="650">
        <g class="jugadores">
          {#each jugadoresData as { src, alturaCm, color, posicion }, index}
            <g class="jugador-container">
              <line 
                x1="10" 
                y1={600 - scaleHeight(alturaCm)} 
                x2={(index * spacing) + 60} 
                y2={600 - scaleHeight(alturaCm)} 
                class="dashed-line" 
                style="stroke: {color}; stroke-dasharray: 5,5; stroke-width: 2;" 
              />
              <image 
                href={src} 
                x={(index * spacing) + 50} 
                y={600 - scaleHeight(alturaCm)} 
                height={scaleHeight(alturaCm)} 
                class="silueta" 
                style="--color: {color};" 
              />
              <text 
                class="altura-text" 
                x={(index * spacing) + 55} 
                y={590 - scaleHeight(alturaCm)} 
                fill={color} 
                font-size="16" 
                font-family="Arial, sans-serif"
              >
                {(alturaCm / 100).toFixed(2)} m
              </text>
            </g>
          {/each}
        </g>
      </svg>
    </div>
  </div>
  <div class="narrativa">
    <p>
      Esta visualización compara la altura de diferentes tipos de jugadores de fútbol, desde niños hasta profesionales en diversas posiciones. 
      Se observa que los defensores y arqueros suelen ser más altos, mientras que los delanteros y mediocampistas presentan una mayor variedad de estaturas. 
      Esto refleja que cada rol en el campo tiene distintas exigencias físicas y que el talento puede encontrarse en cualquier altura.
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
        {#each jugadoresData as { nombre, alturaCm, posicion }}
          <tr>
            <td>{nombre}</td>
            <td>{(alturaCm / 100).toFixed(2)} m</td>
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
      margin-top: 1rem; /* Antes tenía padding-top, lo cambiamos a margin-top más chico */
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
    margin-top: -80px;
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

  .narrativa {
      max-width: 800px;
      margin: 1rem auto;
      padding: 1rem;
      color: #f1f1f1;
      font-family: 'Roboto', sans-serif;
      text-align: center;
      line-height: 1.6;
    }

  .cuadro-comparativo {
      max-width: 800px;
      margin: 2rem auto;
      padding: 1rem;
      background-color: #transparent;
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

  </style>
</main>
