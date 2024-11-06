<!-- Script JS -->
<script>
  import { onMount, onDestroy } from 'svelte';
  import * as d3 from 'd3';

  // Importo las imágenes
  import image1 from './images/familiamasunhijo.png';
  import image2 from './images/familiaunhijo.png';
  import image3 from './images/regla.png';

  // Datos para cada año
  let data1 = [23, 35, 45, 56, 50, 43, 38, 32, 28, 25];
  let data2 = [21, 33, 42, 54, 63, 71, 77, 84, 92, 98];
  let años = [2014, 2015, 2016, 2017, 2018, 2019, 2020, 2021, 2022, 2023];

  // Configuro escalas de D3
  let scaleHeight = d3
    .scaleLinear()
    .domain([0, d3.max([...data1, ...data2])])
    .range([105, 500]);

  // Índices actuales para iterar sobre los datos
  let currentIndex = 0;
  let currentIndex2 = 0;

  // Variable reactiva para los años
  $: currentText = años[currentIndex];

  // Variable reactiva para el estilo del texto
  $: textStyle = currentText >= 2017 
    ? 'font-size: 50px; color: #9A0000; font-family:"Hiromisake"' // Estilo especial para años 2017 en adelante
    : 'font-size: 45px; color: #000000; font-family:"Hiromisake"'; // Estilo normal para los años anteriores a 2017

    // Variable para mostrar la anotación
  $: showAnnotation = currentText >= 2017 && currentText <= 2023;

  // Variables reactivas para las imágenes
  $: currentImage = {
    src: image1,
    alt: "Familia un hijo",
    width: scaleHeight(data1[currentIndex]),
    height: scaleHeight(data1[currentIndex])
  };

  $: currentImage2 = {
    src: image2,
    alt: "Familia mas de un hijo",
    width: scaleHeight(data2[currentIndex2]),
    height: scaleHeight(data2[currentIndex2])
  };

  const interval = 1500; // 1.5 segundos estándar
  let intervalId;

  function startInterval() {
    intervalId = setInterval(() => {
      currentIndex = (currentIndex + 1) % data1.length;
      currentIndex2 = (currentIndex2 + 1) % data2.length;

      // Verifica si el año actual es 2017 o posterior y pausa por más tiempo
      if (años[currentIndex] === 2017) {
        clearInterval(intervalId);
        setTimeout(() => {
          startInterval(); // Reinicia el intervalo después de la pausa
        }, 3000); // Tiempo de pausa más largo para 2017 (3 segundos)
      }
    }, interval);
  }

  onMount(() => {
    startInterval(); // Inicia el intervalo al montar el componente
  });

  onDestroy(() => {
    if (intervalId) clearInterval(intervalId); // Limpia el intervalo al desmontar el componente
  });
</script>


<main class="main">
  <div class="titulos" style="background-color: #9A0000; height: 170px"> 
    <h1 class="titulo">CAMBIO GENERACIONAL EN LA NATALIDAD DE CHINA</h1>
    <h3 class="subtitulo">Encuestas a empleados de una empresa en Hong Kong revelan el impacto de la nueva política<br> fiscal sobre el segundo hijo, provocando la reducción del tamaño en las familias.</h3>
  </div>
  
  <div class="años" style="height: 80px; display: flex; justify-content: center; align-items: center; position: relative; margin-bottom: 15px; font-family:'Hiromisake'">
    <p style={textStyle}>{currentText}</p> <!-- Muestra el año con el estilo condicional -->
  </div>

  <div class="anotacion">
    {#if showAnnotation}
      <div style="position:absolute; margin-left: auto; text-align:justify; color: #9A0000; transform: translateY(-55px)">
        <p>[2017: introducción de la política de <br> impuestos sobre el segundo hijo]</p>
      </div>
    {/if}
  </div>
  
  <div class="originalgraph" style=" position: relative; overflow: hidden;">
    <!-- Imagen de la regla como fondo -->
    <div style="position: absolute; top: 0px; left: 145px; width: auto; height: auto; z-index: 1;">
      <img src={image3} alt="Regla" style="width: 100%; height: 530px; opacity: 0.7;">
    </div>
  
    <!-- Contenedor de la primera imagen -->
    <div style="width: 500px; height: 500px; justify-content: center; display: flex; align-items: flex-end; position: relative; z-index: 2; left:-40px; top: 10px">
      <img 
        src={currentImage.src} 
        alt={currentImage.alt} 
        style={`overflow: hidden; width: ${currentImage.width}px; height: ${currentImage.height}px;`}
      />
    </div>
  
    <!-- Contenedor de la segunda imagen -->
    <div style="width: 500px; height: 500px; justify-content: center; display: flex; align-items: flex-end; position: relative; z-index: 2; left: 40px; top:10px">
      <img 
        src={currentImage2.src} 
        alt={currentImage2.alt} 
        style={`overflow: hidden; width: ${currentImage2.width}px; height: ${currentImage2.height}px;`}
      />
    </div>
  </div>
  

  <div class="flourishgraphs" style="height: 750px">
    <div class="flourish-embed flourish-chart" data-src="visualisation/20145026" style="width:48%; margin-right:auto">
      <script src="https://public.flourish.studio/resources/embed.js">
      </script>
      <div class="graphstitle" style="text-align: center">
        <p><br>Tendencias de Natalidad en China: el impacto de la ley <br>de impuestos sobre el segundo hijo.</p>
      </div>
      <noscript>
        <img src="https://public.flourish.studio/visualisation/20145026/thumbnail" width="100%" alt="chart visualization" />
      </noscript>
    </div>

    <div class="flourish-embed flourish-chart" data-src="visualisation/19628907" style="width: 48%; margin-left:auto">
      <script src="https://public.flourish.studio/resources/embed.js">
      </script>
      <div class="graphstitle" style="text-align: center">
        <p><br>El antes y el después de la ley: como avanzó la brecha <br>numérica desde el impuesto.</p>
      </div>
      <noscript>
        <img src="https://public.flourish.studio/visualisation/19628907/thumbnail" width="100%" alt="chart visualization" />
      </noscript>
  </div>
  </div>

  <div class="footer" style="text-align: center; padding: 5px;">
    <p>Desarrollado por <br>Astrada Pujato Felicitas & Martina Monastra</p>
    <div class="socials" style="display: flex; justify-content: center; gap: 60px; align-items: center;">
      
      <div>
        <a href="https://github.com/felicitasastrada" target="_blank" style="margin-right: 10px;">
          <img src="images/github.png" alt="GitHub" style="width: 35px; height: 35px;">
        </a>
        <a href="https://www.linkedin.com/in/felicitas-astrada/" target="_blank">
          <img src="images/linkedin.png" alt="LinkedIn" style="width: 35px; height: 35px;">
        </a>
      </div>

      <div>
        <a href="https://github.com/Martinamonastra20" target="_blank" style="margin-right: 10px;">
          <img src="images/github.png" alt="GitHub" style="width: 35px; height: 35px;">
        </a>
        <a href="https://www.linkedin.com/in/martina-monastra-aab85a286/" target="_blank">
          <img src="images/linkedin.png" alt="LinkedIn" style="width: 35px; height: 35px;">
        </a>
      </div>
    </div>
  </div>
  

</main>

<style>

  @font-face {
    font-family: 'Hiromisake';
    src: url('fonts/HIROMISAKE.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
  }

  @font-face {
    font-family: 'Sequel Sans Light Disp';
    src: url('fonts/Sequel Sans Light Disp.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
  }

  .main{
  max-width: 100%;
  background-color: #FBFAF7;}
  
  .titulos{
    text-align: center;
    color: #FBFAF7;
    padding: 10px;}

.titulo{
    font-family: 'Hiromisake';
    font-size: 50px;}
  
  .subtitulo{
    font-family: Sequel Sans Light Disp;
    text-align: center;}

  .años{
    margin-bottom: 10px;
  }

  .anotacion{
    font-family: 'Sequel Sans Light Disp';
    font-weight: 600;
    font-size: 20px;
  }
    
  .originalgraph{
    display: flex;
    align-items: end;
    justify-content: center;
    padding: 20px;
    margin-bottom: 20px;}
  
  p{
    transition: font-size 0.5s, color 0.5s;
    padding: 10px;
    max-width: 100%;}

  img{
    max-width: 100%;
    height: auto;
    transition: width 0.5s, height 0.5s;
    object-fit: cover;}

  .graphstitle{
    font-family: 'Sequel Sans Light Disp';
    font-weight: 600;
    font-size: 26px;
    display: flex;
    justify-content: space-evenly;
    padding: 10px;}

  .flourishgraphs{
    display: flex ;
    padding: 20px;}

  .footer{
    font-size: 18px;
    text-align: center;
    background-color: #9A0000;
    color: #FBFAF7;
    font-family: 'Sequel Sans Light Disp';}

</style>