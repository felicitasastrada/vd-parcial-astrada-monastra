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

  let scalePosition = d3
    .scaleLinear()
    .domain([0, data1.length - 1])
    .range([50, 450]); 

  // Índices actuales para iterar sobre los datos
  let currentIndex = 0;
  let currentIndex2 = 0;

  // Variable reactiva para los años
  $: currentText = años[currentIndex];

  // Variable reactiva para el estilo del texto
  $: textStyle = currentText >= 2017 
    ? 'font-size: 45px; color: #9A0000;' // Estilo especial para años 2017 en adelante
    : 'font-size: 40px; color: #000000;'; // Estilo normal para los años anteriores a 2017

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
    <h1 class="titulo">Natalidad en China</h1>
    <h3 class="subtitulo">Como las políticas a través de los años afectaron la cantidad <br> de hijos por familia en la población China</h3>
  </div>
  
  <div class="años" style="display: flex; justify-content: center; align-items: center; position: relative; margin-bottom: 10px;">
    <p style={textStyle}>{currentText}</p> <!-- Muestra el año con el estilo condicional -->
  </div>

  <div class="anotacion" style="font-size:18px">
    {#if showAnnotation}
      <div style="position:absolute; margin-left: auto; text-align: left; color: #9A0000;">
        <p>*A partir de 2017, se observa un cambio significativo <br>en las tendencias gracias a la nueva ley*</p>
      </div>
    {/if}
  </div>
  
  <div class="originalgraph" style="height: 530px">
    <div style="width: 500px; justify-content:center; display:flex">
      <img 
        src={currentImage.src} 
        alt={currentImage.alt} 
        style={`overflow:hidden; width: ${currentImage.width}px; height: ${currentImage.height}px;`}
      />
    </div>

    <div style="margin-left: 20px; margin-right: 20px">
      <img src={image3} 
      alt='' />
    </div>

    <div style="width: 500px; justify-content:center; display:flex">
      <img 
        src={currentImage2.src} 
        alt={currentImage2.alt} 
        style={`overflow:hidden; width: ${currentImage2.width}px; height: ${currentImage2.height}px;`}
      />
    </div>
  </div>

  <div class="flourishgraphs" style="height: 750px">
  <div class="flourish-embed flourish-chart" data-src="visualisation/20145026" style="width:48%; margin-right:auto">
    <script src="https://public.flourish.studio/resources/embed.js">
    </script>
    <div class="graphstitle" style="text-align: center">
      <h5><br>Tendencias en la cantidad de hijos:<br> Cambio hacia familias con un solo hijo</h5>
    </div>
    <noscript>
      <img src="https://public.flourish.studio/visualisation/20145026/thumbnail" width="100%" alt="chart visualization" />
    </noscript>
  </div>

    <div class="flourish-embed flourish-chart" data-src="visualisation/19628907" style="width: 48%; margin-left:auto">
      <script src="https://public.flourish.studio/resources/embed.js">
      </script>
      <div class="graphstitle" style="text-align: center">
        <h5><br>Desbalance creciente entre familias <br> con uno y más hijos en China</h5>
      </div>
      <noscript>
        <img src="https://public.flourish.studio/visualisation/19628907/thumbnail" width="100%" alt="chart visualization" />
      </noscript>
  </div>
  </div>

  <div class= "footer" style="height: 50px;"> 
      <p>Astrada Pujato Felicitas & Martina Monastra</p>
  </div>

</main>

<style>

  @import url('https://fonts.googleapis.com/css2?family=Fira+Sans:ital,wght@0,400;0,500;0,600;1,100;1,400;1,500;1,600&display=swap');

  .main{
  max-width: 100%;
  background-color: #FBFAF7;}
  
  .titulos{
    text-align: center;
    color: #FBFAF7;
    padding: 20px;
    font-family: "Fira Sans", sans-serif;
    font-weight: 400;
    font-style: normal;}
  
  .subtitulo{
    text-align: center;}

  .años{
    margin-bottom: 10px;
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
    max-width: 100%;
    font-family: "Fira Sans", sans-serif;
    font-weight: 400;
    font-style: normal;}

  img{
    max-width: 100%;
    height: auto;
    transition: width 0.5s, height 0.5s;
    object-fit: cover;}

  .graphstitle{
    display: flex;
    justify-content: space-evenly;
    padding: 10px;
    font-family: "Fira Sans", sans-serif;
    font-weight: 200;
    font-style: normal;}

  .flourishgraphs{
    display: flex ;
    padding: 20px;}

  .footer{
    text-align: center;
    background-color: #9A0000;
    color: #FBFAF7;
    font-family: "Fira Sans", sans-serif;
    font-weight: 250;
    font-style: normal; }

</style>