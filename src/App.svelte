<!-- Script JS -->
<script>
  
  import { onMount, onDestroy } from 'svelte';

  //Importo las imagenes
  import image1 from './images/familiaunhijo.png';
  import image2 from './images/familiamasunhijo.png';
  import image3 from './images/regla.png';

  //Genero un array para la primera imagen con el nombre, el ancho y alto
  const images = [
    { src: image1, alt: "Familia un hijo", width: 105, height: 105 },
    { src: image1, alt: "Familia un hijo", width: 165, height: 165 },
    { src: image1, alt: "Familia un hijo", width: 210, height: 210 },
    { src: image1, alt: "Familia un hijo", width: 270, height: 270 },
    { src: image1, alt: "Familia un hijo", width: 315, height: 315 },
    { src: image1, alt: "Familia un hijo", width: 355, height: 355 },
    { src: image1, alt: "Familia un hijo", width: 385, height: 385 },
    { src: image1, alt: "Familia un hijo", width: 420, height: 420 },
    { src: image1, alt: "Familia un hijo", width: 460, height: 460 },
    { src: image1, alt: "Familia un hijo", width: 490, height: 490 }
  ];

  //Genero un array para la segunda imagen con el nombre, el ancho y alto
  const images2 = [
    { src: image2, alt: "Familia mas de un hijo", width: 115, height: 115 },
    { src: image2, alt: "Familia mas de un hijo", width: 175, height: 175 },
    { src: image2, alt: "Familia mas de un hijo", width: 225, height: 225 },
    { src: image2, alt: "Familia mas de un hijo", width: 280, height: 280 },
    { src: image2, alt: "Familia mas de un hijo", width: 250, height: 250 },
    { src: image2, alt: "Familia mas de un hijo", width: 215, height: 215 },
    { src: image2, alt: "Familia mas de un hijo", width: 190, height: 190 },
    { src: image2, alt: "Familia mas de un hijo", width: 160, height: 160 },
    { src: image2, alt: "Familia mas de un hijo", width: 140, height: 140 },
    { src: image2, alt: "Familia mas de un hijo", width: 125, height: 125 }
  ];

  const texts = [
    { content: "2014", fontSize: 40, color: "#000000" },
    { content: "2015", fontSize: 40, color: "#000000" },
    { content: "2016", fontSize: 40, color: "#000000" },
    { content: "2017", fontSize: 40, color: "#000000" },
    { content: "2018", fontSize: 40, color: "#000000" },
    { content: "2019", fontSize: 40, color: "#000000" },
    { content: "2020", fontSize: 40, color: "#000000" },
    { content: "2021", fontSize: 40, color: "#000000" },
    { content: "2022", fontSize: 40, color: "#000000" },
    { content: "2023", fontSize: 40, color: "#000000" },
  ]

  //Index actual
  let currentIndex = 0;
  let currentIndex2 = 0;
  let currentIndex3 = 0;

  $: currentImage = images[currentIndex];
  $: imageStyle = `width: ${currentImage.width}px; height: ${currentImage.height}px;`;

  $: currentImage2 = images2[currentIndex2];
  $: imageStyle2 = `width: ${currentImage2.width}px; height: ${currentImage2.height}px;`;

  $: currentText = texts[currentIndex];
  $: textStyle = `font-size: ${currentText.fontSize}px; color: ${currentText.color};`;

  //Intervalo de tiempo entre imagen 
  const interval = 1500; // 3 seconds

  let intervalId;

  onMount(() => {
    //Seteo el intervalo para cambiar la imagen 1
    intervalId = setInterval(() => {
      currentIndex = (currentIndex + 1) % images.length;
    }, interval);
  });

  onMount(() => {
    //Seteo el intervalo para cambiar la imagen 2
  intervalId2 = setInterval(() => {
      currentIndex2 = (currentIndex2 + 1) % images2.length;
    }, interval);
  });

  onMount(() => {
    // Set up the interval to change the text
    intervalId3 = setInterval(() => {
      currentIndex3 = (currentIndex3 + 1) % texts.length;
    }, interval);
  });

  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });

  onDestroy(() => {
    if (intervalId2) clearInterval(intervalId2);
  });

  onDestroy(() => {
    // Clear the interval when the component is destroyed
    if (intervalId3) clearInterval(intervalId3);
  });

</script>

<main class="main">

  <div class="titulos" style="background-color: #9A0000; height: 170px"> 
      <h1 class="titulo">Natalidad en China</h1>
      <h3 class="subtitulo">Como las políticas a través de los años afectaron la cantidad <br> de hijos por familia en la población China</h3>
    </div>
    
    <div class="años" style="display:flex ; justify-content:center">
      <p style={textStyle}>{currentText.content}</p>
    </div>

    <div class= "originalgraph" style="height: 530px;">

      <div style="width: 500px; justify-content:center; display:flex"> 
        <img 
      src={currentImage.src} 
      alt={currentImage.alt} 
      style={imageStyle}
      />  
      </div>


      <div style="margin-left: 20px; margin-right: 20px">
         <img src={image3}
         alt=''
         >
      </div>


      <div style="width:500px; justify-content:center; display:flex"> 
        <img 
        src={currentImage2.src} 
        alt={currentImage2.alt} 
        style={imageStyle2}
        />  
      </div>

    </div>


    <div class="flourishgraphs" style="height: 750px;">

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