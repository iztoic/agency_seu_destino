<template>
    <section id="section-about" class="section-about">
    <div class="container">
        <div class="row clearfix">
            
            <!--Content Column-->
            <div class="content-column col-md-6 col-sm-12 col-xs-12">
                <div class="inner-column">
                    <div class="sec-title">
                        <div class="title">Sobre Nós</div>
                        <h2>Inovação<br>Transformação<br>Impacto</h2>
                    </div>
                    <div class="text">Somos uma agência criativa que trabalha com sua maior paixão: O desenvolvimento de soluções estratégicas para conectar sua marca com o consumidor de forma humanizada. <br> Somos uma equipe que traz o design e o marketing de um jeito unico para dar forma a um único objetivo: Tornar sua marca ainda mais relevante para as pessoas, ajudando a construir seu sonho.</div>
                    </div>
            </div>
            
            <!-- Image Column -->
    <div class="image-column col-md-6 col-sm-12 col-xs-12">
      <div class="inner-column " data-wow-delay="0ms" data-wow-duration="1500ms">
        <div class="image">
          <img :src="currentImage" alt="">
        </div>
      </div>
    </div>
        </div>
    </div>
</section>
</template>

<script>
export default {
  name: "SectionAbout",
  data() {
    return {
      images: [
        "src/assets/img/people_drone.jpg",
        "src/assets/img/wine_bar.jpg",
        "src/assets/img/drone_tecnology.jpg",
        "src/assets/img/restaurant_drink.jpg",
      ],
      currentImageIndex: 0,
    };
  },
  computed: {
    currentImage() {
      return this.images[this.currentImageIndex];
    },
  },
  mounted() {
    // Inicia a troca automática de imagens
    this.startImageRotation();
  },
  methods: {
    startImageRotation() {
      // Troca a imagem a cada 5 segundos (5000 milissegundos)
      setInterval(() => {
        this.currentImageIndex = (this.currentImageIndex + 1) % this.images.length;
      }, 5000);
    },
  },
};
</script>

<style scoped>
.section-about{
border: 3px solid #d7a449;
border-right: 3px solid transparent;
position:relative;
padding-top:80px;

}

.section-about .content-column{
position:relative;
}

.section-about .content-column .inner-column{
position:relative;
padding-right:100px;
align-items: flex-start;
}

.section-about .content-column .text{
position:relative;
color:#777777;
padding: 10px;
font-size:16px;
line-height:1.8em;
margin-bottom:20px;
background-color: rgba(41, 41, 41, 0.7);
border-radius: 10px;
max-width: 400px; /* Limita a largura máxima para melhor legibilidade */
margin-right: 20px; /* Adiciona espaço à direita para separar do lado da imagem */
}

.section-about .content-column .text::before {
  content: '\201C'; /* Adicione aspas duplas no início do texto */
  font-size: 40px;
  color: #d7a449;
  position: absolute;
  left: -20px;
  top: -10px;
}

.section-about .content-column .text::after {
  content: '\201D'; /* Adicione aspas duplas no final do texto */
  font-size: 40px;
  color: #d7a449;
  position: absolute;
  right: -20px;
  bottom: -10px;
}

/* Adicione um efeito de sobreposição para destacar o texto */
.section-about .content-column .text:hover {
  color: #d7a449;
}

/* Destaque o título da seção */
.section-about .sec-title .title {
  color: #d7a449;
  font-size: 34px; /* Aumente o tamanho do título */
  font-weight: 700;
  margin-bottom: 15px;
}

/* Adicione um fundo sutil para o título da seção */
.section-about .sec-title .title:before {
  content: '';
  position: absolute;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 2px;
  background-color: #d7a449;
}

/* Ajuste a estilização do subtítulo */
.section-about .sec-title2 {
  color: #fff;
  font-size: 16px;
  font-weight: 700;
}

/* Adicione um fundo sutil à área do texto */
.section-about .content-column {
  padding: 40px;
  border-radius: 10px;
  margin-bottom: 40px;
}

/* Adicione um espaçamento adicional ao título para melhorar a aparência */
.section-about .sec-title {
  margin-bottom: 20px;
}


.section-about .content-column .email{
position:relative;
color:#252525;
font-weight:700;
margin-bottom:20px;
}

.section-about .image-column{
position:relative;
margin-bottom:50px;
}

.section-about .image-column .inner-column{
position:relative;
padding:40px 40px 0px 0px;
margin-left:50px;
}

.section-about .image-column .inner-column:after{
position:absolute;
content:'';
right:0px;
top:0px;
left:40px;
bottom:100px;
z-index:-1;
border:2px solid #d7a449;
}

.section-about .image-column .inner-column .image{
position:relative;
overflow: hidden; /* Garante que a imagem não ultrapasse a borda do contêiner */
  border-radius: 10px; /* Borda arredondada para um visual mais suave */
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Adiciona uma sombra sutil */
  width: 100%; /* Definir a largura do contêiner da imagem */
  height: 300px;
}

.section-about .image-column .inner-column .image:before{
position:absolute;
content:'';
left:-50px;
bottom:-50px;
width:299px;
height:299px;
}
/* Adiciona estilos ao contêiner da imagem durante a transição */
.section-about .image-column .inner-column .image img {
  position: relative;
  width: 100%;
  display: block;
  transition: opacity 1.5s ease-in-out, transform 1.5s ease-in-out; /* Adicione a transição de opacidade e transformação */
  object-fit: cover;
  border-radius: 10px;
}

/* Define a transformação durante a transição */
.section-about .image-column .inner-column .image img.fade-enter,
.section-about .image-column .inner-column .image img.fade-leave-to {
  opacity: 1;
  transform: scale(1.9); /* Aumenta a escala para criar um efeito de distorção */
}

.section-about .image-column .inner-column .image img.fade-enter-active,
.section-about .image-column .inner-column .image img.fade-leave-active {
  opacity: 0;
  transform: scale(1); /* Retorna à escala original durante a transição */
}

.section-about .image-column .inner-column .image .overlay-box{
position:absolute;
left:40px;
bottom:48px;
}

.section-about .image-column .inner-column .image .overlay-box .year-box{
position:relative;
color:#252525;
font-size:24px;
font-weight:700;
line-height:1.4em;
padding-left:125px;
}

.section-about .image-column .inner-column .image .overlay-box .year-box .number{
position:absolute;
left:0px;
top:0px;
width:110px;
height:110px;
color:#d7a449;
font-size:68px;
font-weight:700;
line-height:105px;
text-align:center;
background-color:#ffffff;
border:1px solid #000000;
}
.section-about .btn-style-three:before {
position: absolute;
content: '';
left: 10px;
top: 10px;
z-index: -1;
right: -10px;
bottom: -10px;
background: url(https://i.ibb.co/DKn55Qz/pattern-1.jpg) repeat;
}
.section-about .btn-style-three:hover {
color: #ffffff;
background: #d7a449;
}
.section-about .btn-style-three {
position: relative;
line-height: 24px;
color: #252525;
font-size: 15px;
font-weight: 700;
background: none;
display: inline-block;
padding: 11px 40px;
background-color: #ffffff;
text-transform: capitalize;
border: 2px solid #d7a449;
font-family: 'Arimo', sans-serif;
}

h2{
  color:#d7a449;
  text-shadow: 3px 3px 0 hsl(0, 0%, 0%, 0.25);
}
.sec-title2{
color:#fff;
}
.sec-title {
position: relative;
padding-bottom: 40px;
}
.sec-title .title {
position: relative;
color: #d7a449;
text-shadow: 3px 3px 0 hsl(0, 0%, 0%, 0.25);
font-size: 18px;
font-weight: 700;
padding-right: 50px;
margin-bottom: 15px;
display: inline-block;
text-transform: capitalize;
}
.sec-title .title:before {
position: absolute;
content: '';
right: 0px;
bottom: 7px;
width: 40px;
height: 1px;
background-color: #bbbbbb;
}



</style>