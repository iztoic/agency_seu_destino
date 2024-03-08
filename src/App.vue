<template>
	<div id="app">
		<div class="main-section">
        		<InteractiveMap />
			<!-- <div class="logo"><img src="src/assets/img/logo/iztoic_logo.png"></div> -->
			<div class="about-text hover-target">Sobre</div>
			<div class="contact-text hover-target">Contato</div>
			<div class="div-inferior">
				<div class="container">
					<div class="row justify-content-center">
						<div class="col-md-4">
							<span class="copyright">Copyright © Iztoic 2023</span>
						</div>
						<div class="col-md-4">
							<ul class="list-inline social-buttons justify-content-center">
								<li><a href="#"><i class="fa fa-twitter"></i></a></li>
								<li><a onclick="window.open('https://www.instagram.com/iztoic', '_blank');">
									<i class="fa fa-instagram"></i></a></li>
								<li><a href="#"><i class="fa fa-linkedin"></i></a></li>
							</ul>
						</div>
						<div class="col-md-4">
							<ul class="list-inline quicklinks">
								<li><a href="#" class="white-and-bold">Politica de Privacidade</a></li>
							</ul>
						</div>
					</div>
				</div>
			</div>
		</div>

		<div class="layerindex page_about-section">
			<div class="about-close hover-target"></div>
			<SectionAbout />
			<SectionTeam />
		</div>

		<div class="layerindex page_contact-section">
			<div class="contact-close hover-target"></div>
			<SectionContact />
		</div>

		<div class='cursor' id="cursor"></div>
		<div class='cursor2' id="cursor2"></div>
		<div class='cursor3' id="cursor3"></div>


		<div @click="showHomepage">
    <div v-if="loading" class="loading">
        <div class="loading-overlay"></div>
        <div class="loading-content">
            <div class="loading-progress" :style="{ width: progress + '%' }"></div>
            <div class="loading-title">
            </div>
        </div>
    </div>
</div>

	</div>
</template>
<script src="https://api.mapbox.com/mapbox-gl-js/v2.14.1/mapbox-gl.js"></script>
<script src="https://unpkg.com/@turf/turf@6/turf.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.11.2/jquery-ui.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.9.0/gsap.min.js"></script>
<script src="https://raw.githubusercontent.com/IronSummitMedia/startbootstrap/gh-pages/templates/agency/js/bootstrap.min.js"></script>
<script>
import InteractiveMap from './components/interactive_map.vue';
import SectionAbout from './components/section_about.vue';
import SectionContact from './components/section_contact.vue';
export default {
	name: 'app',
	components: {
		SectionAbout,
		InteractiveMap,
		SectionContact,
	},

	data() {
    return {
      loading: false,
      progress: 0,
	  
    };
  },

  methods: {
    simulateLoader() {
      const loaderInterval = setInterval(() => {
        this.progress += this.getRandomArbitrary(10, 25);

        if (this.progress >= 75) {
          clearInterval(loaderInterval);
          this.progress = 100;

          // Adicione um pequeno atraso antes de esconder o loader
          setTimeout(() => {
            this.showHomepage();
          }, 7000);
        }
      }, this.getRandomArbitrary(100, 500));
    },
    getRandomArbitrary(min, max) {
      return Math.random() * (max - min) + min;
    },
    showHomepage() {
      this.loading = false;
    },
  },

	mounted() {
		this.simulateLoader();

		document.addEventListener("DOMContentLoaded", function () {
			// Page cursors
			var cursor = document.getElementById("cursor");
			var cursor2 = document.getElementById("cursor2");
			var cursor3 = document.getElementById("cursor3");

			document.body.addEventListener("mousemove", function (event) {
				cursor.style.left = event.clientX + "px";
				cursor.style.top = event.clientY + "px";
				cursor2.style.left = event.clientX + "px";
				cursor2.style.top = event.clientY + "px";
				cursor3.style.left = event.clientX + "px";
				cursor3.style.top = event.clientY + "px";
			});

			function addHover() {
				cursor2.classList.add("hover");
				cursor3.classList.add("hover");
			}

			function removeHover() {
				cursor2.classList.remove("hover");
				cursor3.classList.remove("hover");
			}

			removeHover();

			var hoverTargets = document.querySelectorAll(".hover-target");

			hoverTargets.forEach(function (element) {
				element.addEventListener("mouseover", addHover);
				element.addEventListener("mouseout", removeHover);
			});

			// About page
			var aboutText = document.querySelector(".about-text");
			var aboutClose = document.querySelector(".about-close");

			aboutText.addEventListener("click", function () {
				document.body.classList.add("about-on");
			});

			aboutClose.addEventListener("click", function () {
				document.body.classList.remove("about-on");
			});

			// Contact page
			var contactText = document.querySelector(".contact-text");
			var contactClose = document.querySelector(".contact-close");

			contactText.addEventListener("click", function () {
				document.body.classList.add("contact-on");
			});

			contactClose.addEventListener("click", function () {
				document.body.classList.remove("contact-on");
			});
		});

		setTimeout(() => {
    // Exibir a div-inferior após 2 segundos
    document.querySelector('.div-inferior').style.display = 'flex';
  }, 500);

		
	},
};
</script>

<style scoped>
@import url('https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css');
@import url('https://fonts.googleapis.com/css?family=Poppins:100,100i,200,200i,300,300i,400,400i,500,500i,600,600i,700,700i,800,800i,900,900i');
@import url('https://fonts.googleapis.com/css?family=Dancing+Script:400,700');
@import url('https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.1.3/css/bootstrap.min.css');
@import url('https://startbootstrap.com/templates/agency/font-awesome-4.1.0/css/font-awesome.min.css');
@import url('https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css');
@import url('https://ajax.googleapis.com/ajax/libs/jqueryui/1.11.2/themes/smoothness/jquery-ui.css');
@import url('https://api.mapbox.com/mapbox-gl-js/v2.14.1/mapbox-gl.css');

body {
	font-family: 'Poppins', sans-serif;
	font-weight: 300;
	font-size: 15px;
	line-height: 1.6;
	color: #fff;
	background-color: #1f2029;
    background-repeat: repeat; /* ou repeat-x / repeat-y conforme necessário */
    background-attachment: fixed; /* ou scroll conforme necessário */
    background-position: center center;
    background-size: cover; /* ou contain conforme necessário */
	overflow: hidden;
	height: 100vh;
	-webkit-transition: all 300ms linear;
	transition: all 300ms linear;
	perspective: 800px;
}

a {
	cursor: pointer;
}

a:hover {
	text-decoration: none;
}

::selection {
	color: #c4c3ca;
	background-color: #000;
}

::-moz-selection {
	color: #c4c3ca;
	background-color: #000;
}

/* #Cursor
================================================== */

.cursor,
.cursor2,
.cursor3 {
	position: fixed;
	border-radius: 50%;
	transform: translateX(-50%) translateY(-50%);
	pointer-events: none;
	left: -100px;
	top: 50%;
	mix-blend-mode: difference;
	-webkit-transition: all 300ms linear;
	transition: all 300ms linear;
}

.cursor {
	background-color: #fff;
	height: 0;
	width: 0;
	z-index: 99999;
}

.cursor2,
.cursor3 {
	height: 36px;
	width: 36px;
	z-index: 99998;
	-webkit-transition: all 0.3s ease-out;
	transition: all 0.3s ease-out
}

.cursor2.hover,
.cursor3.hover {
	-webkit-transform: scale(2) translateX(-25%) translateY(-25%);
	transform: scale(2) translateX(-25%) translateY(-25%);
	border: none
}

.cursor2.hover {
	background: rgba(255, 255, 255, 1);
	border-color: transparent;
}

@media screen and (max-width: 1200px) {

	.cursor,
	.cursor2,
	.cursor3 {
		display: none
	}
}

/* #Primary style
================================================== */

.main-section {
	position: relative;
	width: 100%;
	display: block;
	overflow: hidden;
	height: 100vh;
	/* background-image: url(src/assets/img/background_page.png); */
	background-size: cover;
	background-position: center;
	transform: scale(1) rotateX(0);
	box-shadow: 0 0 40px rgba(0, 0, 0, 0.2);
	-webkit-transition: all 300ms linear;
	transition: all 300ms linear;
	border: 3px solid #d7a449;
	-webkit-transition-delay: 400ms;
	transition-delay: 400ms;
	transform-origin: center top;
}

.main-section h1 {
	color: #fff;
	font-size: 5vw;
	line-height: 1;
	font-weight: 900;
}

.main-section .dancing {
	font-family: 'Dancing Script', cursive;
	letter-spacing: 1px;
	color: #866BAF;
	font-size: 26px;
	line-height: 1;
	font-weight: 700;
}

@media screen and (max-width: 580px) {
	.main-section .dancing {
		font-size: 18px;
	}
}

.main-section p {
	font-size: 15px;
	line-height: 20px;
	font-weight: 500;
	color: #fff;
	letter-spacing: 1px;
}

.main-section p span {
	margin-left: 15px;
	color:#d7a449;
	margin-right: 15px;
	position: relative;
	display: inline-block;
	cursor: pointer;
	padding-bottom: 35px;
}

.main-section p span:before {
	position: absolute;
	content: '';
	bottom: 10px;
	left: 50%;
	height: 30px;
	width: 30px;
	margin-left: -15px;
	z-index: -1;
	background-repeat: no-repeat;
	background-position: center;
	background-size: 30px 30px;
	background-image: url('src/assets/img//down.svg');
	opacity: 0.2;
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
}

.main-section p span:hover:before {
	opacity: 1;
	bottom: 5px;
}

body.about-on .main-section {
	transform: scale(0.75);
	-webkit-transition-delay: 0ms;
	transition-delay: 0ms;
}

body.contact-on .main-section {
	transform: scale(0.75);
	-webkit-transition-delay: 0ms;
	transition-delay: 0ms;
}


body.page_clients-on .main-section {
	transform: scale(0.75);
	-webkit-transition-delay: 0ms;
	transition-delay: 0ms;
}

body.page_services-on .main-section {
	transform: rotateX(-10deg);
	-webkit-transition-delay: 0ms;
	transition-delay: 0ms;
}

body.page_plans-on .main-section {
	transform: rotateX(-10deg);
	-webkit-transition-delay: 0ms;
	transition-delay: 0ms;
}

body.page_portifolio-on .main-section {
	transform: rotateX(-10deg);
	-webkit-transition-delay: 0ms;
	transition-delay: 0ms;
}


body.page_clients-on .main-section {
	transform: rotateX(-10deg);
	-webkit-transition-delay: 0ms;
	transition-delay: 0ms;
}
.about-text {
	position: absolute;
	font-size: 17px;
	line-height: 20px;
	font-weight: 700;
	color:#d7a449;
	letter-spacing: 2px;
	top: 50%;
	left: 40px;
	cursor: pointer;
	z-index: 2;
	-webkit-writing-mode: vertical-lr;
	writing-mode: vertical-lr;
	transform: translateY(-50%);
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
}

.about-text:before {
	position: absolute;
	content: '';
	top: 50%;
	left: -25px;
	height: 30px;
	width: 30px;
	margin-top: -15px;
	z-index: -1;
	background-repeat: no-repeat;
	background-position: center;
	background-size: 30px 30px;
	background-image: url('src/assets/img/left.svg');
	opacity: 0.2;
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
}

.about-text:hover:before {
	opacity: 1;
	left: -30px;
}

.page_about-section {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	display: block;
	overflow: hidden;
	height: 100vh;
	background-color: rgba(31, 32, 41, 1);
	transform: translateX(-100%);
	-webkit-transition: all 300ms linear;
	transition: all 300ms linear;
	z-index: 10;
}

body.about-on .page_about-section {
	transform: translateX(0);
	-webkit-transition-delay: 400ms;
	transition-delay: 400ms;
}

.about-close {
	position: absolute;
	top: 20px;
	right: 20px;
	width: 30px;
	display: block;
	overflow: hidden;
	height: 30px;
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
	background-repeat: no-repeat;
	background-position: center;
	background-size: 36px 36px;
	background-image: url('src/assets/img/close.svg');
	cursor: pointer;
	z-index: 11;
}

.about-close:hover {
	transform: rotate(90deg);
}

.center-section {
	position: absolute;
	width: 100%;
	left: 0;
	top: 50%;
	transform: translateY(-50%);
	z-index: 1;
}

.page_about-section img {
	width: 100px;
	height: 100px;
	border-radius: 4px;
	display: block;
	margin: 0 auto;
	box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

.page_about-section p span {
	font-family: 'Dancing Script', cursive;
	letter-spacing: 1px;
	color: #866BAF;
	font-size: 25px;
	font-weight: 400;
}

.contact-text {
	position: absolute;
	font-size: 17px;
	line-height: 20px;
	font-weight: 700;
	color:#d7a449;
	letter-spacing: 2px;
	top: 50%;
	right: 40px;
	cursor: pointer;
	z-index: 2;
	-webkit-writing-mode: vertical-lr;
	writing-mode: vertical-lr;
	transform: translateY(-50%) rotate(180deg);
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
}

.contact-text:before {
	position: absolute;
	content: '';
	top: 50%;
	left: -25px;
	height: 30px;
	width: 30px;
	margin-top: -15px;
	z-index: -1;
	background-repeat: no-repeat;
	background-position: center;
	background-size: 30px 30px;
	background-image: url('src/assets/img/left.svg');
	opacity: 0.2;
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
}

.contact-text:hover:before {
	opacity: 1;
	left: -30px;
}

.page_clients-section{
	background-image: url('src/assets/img/wallpaper_clients.jpg');
	background-size: cover;
	background-position: center;
}

.page_contact-section {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	display: block;
	overflow: hidden;
	height: 100vh;
	background-color: rgba(31, 32, 41, 1);
	transform: translateX(100%);
	-webkit-transition: all 300ms linear;
	transition: all 300ms linear;
	z-index: 10;
}

body.contact-on .page_contact-section {
	transform: translateX(0);
	-webkit-transition-delay: 400ms;
	transition-delay: 400ms;
}

.contact-close {
	position: absolute;
	top: 20px;
	right: 20px;
	width: 30px;
	display: block;
	overflow: hidden;
	height: 30px;
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
	background-repeat: no-repeat;
	background-position: center;
	background-size: 36px 36px;
	background-image: url('src/assets/img/close.svg');
	cursor: pointer;
	z-index: 11;
}

.contact-close:hover {
	transform: rotate(90deg);
}

.page_contact-section a {
	margin: 0 auto;
	font-size: 32px;
	font-weight: 900;
	letter-spacing: 1px;
	color: #fff;
	display: inline-block;
}

.page_contact-section .social a {
	margin-left: 8px;
	margin-right: 8px;
	font-size: 15px;
	font-weight: 600;
	letter-spacing: 1px;
	color: #866BAF;
}

@media screen and (max-width: 580px) {
	.page_contact-section a {
		font-size: 17px;
	}

	.page_contact-section .social a {
		margin-left: 3px;
		margin-right: 3px;
		font-size: 13px;
		letter-spacing: 0;
	}
}

.page_services-section,
.page_plans-section,
.page_clients-section,
.page_portifolio-section {
	position: fixed;
	top: 100%;
	left: 0;
	width: 100%;
	height: 100vh;
	display: flex;
	flex-direction: column;
	justify-content: flex-start;
	align-items: center;
	background-color: rgba(31, 32, 41, 1);
	-webkit-transition: all 300ms linear;
	transition: all 300ms linear;
	z-index: 10;
}

.page_services-close,
.page_plans-close,
.page_clients-close,
.page_portifolio-close {
	position: absolute;
	top: 20px;
	right: 20px;
	width: 30px;
	display: block;
	overflow: hidden;
	height: 30px;
	-webkit-transition: all 200ms linear;
	transition: all 200ms linear;
	background-repeat: no-repeat;
	background-position: center;
	background-size: 36px 36px;
	background-image: url('src/assets/img/close.svg');
	cursor: pointer;
	z-index: 11;
}

.page_services-close:hover,
.page_plans-close:hover,

.page_clients-close:hover,
.page_portifolio-close:hover {
	transform: rotate(90deg);
}

body.page_services-on .page_services-section {
	top: 0;
	-webkit-transition-delay: 400ms;
	transition-delay: 400ms;
	border: 3px solid #d7a449;
	border-right: 3px solid transparent;
}

body.page_plans-on .page_plans-section {
	top: 0;
	-webkit-transition-delay: 400ms;
	transition-delay: 400ms;
	border: 3px solid #d7a449;
	border-right: 3px solid transparent;
}

body.page_portifolio-on .page_portifolio-section {
	top: 0;
	-webkit-transition-delay: 400ms;
	transition-delay: 400ms;
}


body.page_clients-on .page_clients-section {
	top: 0;
	-webkit-transition-delay: 400ms;
	transition-delay: 400ms;
	border: 3px solid #d7a449;
}

.page_services-section img,
.page_plans-section img,
.page_clients-section img,
.page_portifolio-section img {
	margin-top: 30px;
	width: 100%;
	height: auto;
	display: block;
	border-radius: 4px;
	box-shadow: 0 0 20px rgba(0, 0, 0, 0.4);
}

.page_services-section h3,
.page_plans-section h3,
.page_clients-section h3,
.page_portifolio-section h3 {
	font-size: 44px;
	line-height: 1.3;
	font-weight: 700;
	letter-spacing: 1px;
	color: #fff;
}

.page_services-section p,
.page_plans-section p,
.page_clients-section p,
.page_portifolio-section p {
	font-size: 14px;
	line-height: 1.7;
	letter-spacing: 1px;
}

.page_services-section p span,
.page_plans-section p span,
.page_clients-section p span,
.page_portifolio-section p span {
	font-size: 20px;
	line-height: 1.3;
	font-weight: 700;
	color: #866BAF;
}

.layerindex {
	overflow-y: auto;
}

header {
	text-align: center;
	color: #fff;
	background-attachment: scroll;
	background-position: center center;
	background-repeat: none;
	-webkit-background-size: cover;
	-moz-background-size: cover;
	background-size: cover;
	-o-background-size: cover;
}

header .intro-text {
	padding-top: 100px;
	padding-bottom: 50px;
}

header .intro-text .intro-lead-in {
	margin-bottom: 25px;
	font-family: sans-serif;
	font-size: 22px;
	font-style: italic;
	line-height: 22px;
	color: #d7a449;
}

header .intro-text .intro-heading {
	margin-bottom: 25px;
	text-transform: uppercase;
	font-family: Montserrat, "Helvetica Neue", Helvetica, Arial, sans-serif;
	font-size: 50px;
	font-weight: 700;
	line-height: 50px;
	color: #d7a449;
}

.page-text{
	color: #d7a449 !important;
}

@media(min-width:768px) {
	header .intro-text {
		padding-top: 300px;
		padding-bottom: 200px;
	}

	header .intro-text .intro-lead-in {
		margin-bottom: 25px;
		font-family: sans-serif;
		font-size: 40px;
		font-style: italic;
		line-height: 40px;
	}

	header .intro-text .intro-heading {
		margin-bottom: 50px;
		text-transform: uppercase;
		font-family: Montserrat, "Helvetica Neue", Helvetica, Arial, sans-serif;
		font-size: 75px;
		font-weight: 700;
		line-height: 75px;
	}
}

.btn-xl {
	padding: 20px 40px;
	border-color: #1ee2e7;
	border-radius: 3px;
	text-transform: uppercase;
	font-family: Montserrat, "Helvetica Neue", Helvetica, Arial, sans-serif;
	font-size: 18px;
	font-weight: 700;
	color: #fff;
	background-color: #1ee2e7;
}

.btn-xl:hover,
.btn-xl:focus,
.btn-xl:active,
.btn-xl.active,
.open .dropdown-toggle.btn-xl {
	border-color: #17d0d5;
	color: #fff;
	background-color: #17d0d5;
}

.btn-xl:active,
.btn-xl.active,
.open .dropdown-toggle.btn-xl {
	background-image: none;
}

.btn-xl.disabled,
.btn-xl[disabled],
fieldset[disabled] .btn-xl,
.btn-xl.disabled:hover,
.btn-xl[disabled]:hover,
fieldset[disabled] .btn-xl:hover,
.btn-xl.disabled:focus,
.btn-xl[disabled]:focus,
fieldset[disabled] .btn-xl:focus,
.btn-xl.disabled:active,
.btn-xl[disabled]:active,
fieldset[disabled] .btn-xl:active,
.btn-xl.disabled.active,
.btn-xl[disabled].active,
fieldset[disabled] .btn-xl.active {
	border-color: #1ee2e7;
	background-color: #1ee2e7;
}

.btn-xl .badge {
	color: #fed136;
	background-color: #fff;
}

footer {
	padding: 25px 0;
	text-align: center;
}

span.copyright {
	text-transform: uppercase;
	text-transform: none;
	font-family: Montserrat, "Helvetica Neue", Helvetica, Arial, sans-serif;
	line-height: 40px;
	color: #fff !important;
}

ul.quicklinks {
	margin-bottom: 0;
	text-transform: uppercase;
	text-transform: none;
	font-family: Montserrat, "Helvetica Neue", Helvetica, Arial, sans-serif;
	line-height: 40px;
}

ul.social-buttons {
	margin-bottom: 0;
}

ul.social-buttons li a {
	display: block;
	width: 40px;
	height: 40px;
	border-radius: 100%;
	font-size: 20px;
	line-height: 40px;
	outline: 0;
	color: #fff;
	background-color: #222;
	-webkit-transition: all .3s;
	-moz-transition: all .3s;
	transition: all .3s;
}

ul.social-buttons li a:hover,
ul.social-buttons li a:focus,
ul.social-buttons li a:active {
	background-color: #1ee2e7;
}

.btn:focus,
.btn:active,
.btn.active,
.btn:active:focus {
	outline: 0;
}

.row {
	align-items: center
}
.copyright {
  font-size: 14px;
}

.div-inferior {
	position: fixed;
	bottom: 0;
	left: 0;
	width: 100%;
	display: none;
	justify-content: center;
	align-items: center;
	padding: 20px;
	z-index: 1000;
}

.social-buttons {
	text-align: center;
}

.div-inferior .container {
	width: 100%;
}

.col-md-4 {
	display: flex;
	justify-content: center;
}

.white-and-bold {
	color: #fff !important;
}
.quicklinks a {
  color: #fff; /* Cor dos links rápidos */
  font-weight: bold;
  text-decoration: none;
}

.quicklinks a:hover {
  text-decoration: underline;
}

.loading {
    position: fixed;
    height: 100vh;
    width: 100vw;
    top: 0px;
    left: 0;
    z-index: 10000;
}

.loading-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: linear-gradient(rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.5)), url('src/assets/portifolio/wine_bar.jpg');
    transition: background-color 0.75s;
    z-index: 1;
}

.loading-content {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    text-align: center;
    color: #fff;
}

.logo {
  position: absolute;
  top: 20px;
  left: 20px;
  z-index: 100;
}

.logo img {
  max-width: 50%;
  height: auto;
  border-radius: 50%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  border: 2px solid #d7a449; /* Adiciona uma borda branca de 2 pixels */
  padding: 5px; /* Adiciona um preenchimento interno para afastar a imagem da borda */
}

::-webkit-scrollbar {
    width: 10px; /* Largura da barra de rolagem */
}

::-webkit-scrollbar-thumb {
    background-color: #d7a449; /* Cor da linha da barra de rolagem */
    border-radius: 6px; /* Borda arredondada da linha */
}


</style>
