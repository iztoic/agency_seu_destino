<template>
  <div>
    <section id="section-contact">
      <h1 class="section-header">Contato</h1>
      <div class="contact-wrapper">
        <form id="contact-form" class="form-horizontal" role="form" @submit.prevent="submitForm">
          <div class="form-group">
            <div class="col-sm-12">
              <input type="text" class="form-control" v-model="formData.name" placeholder="NOME" name="name" required>
            </div>
          </div>
          <div class="form-group">
            <div class="col-sm-12">
              <input type="email" class="form-control" v-model="formData.email" placeholder="EMAIL" name="email" required>
            </div>
          </div>
          <textarea class="form-control" rows="10" v-model="formData.message" placeholder="MENSAGEM" name="message" required></textarea>
          <button class="btn btn-primary send-button" id="submit" type="submit" value="SEND">
            <div class="alt-send-button">
              <i class="fa fa-paper-plane"></i><span class="send-text">ENVIAR</span>
            </div>
          </button>
        </form>
        <div class="direct-contact-container">
          <ul class="contact-list">
            <li class="list-item"><i class="fa fa-map-marker fa-2x"><span class="contact-text place">{{ contactInfo.location }}</span></i></li>
            <li class="list-item"><i class="fa fa-phone fa-2x"><span class="contact-text phone"><a :href="'tel:' + contactInfo.phone" >{{ contactInfo.phone }}</a></span></i></li>
            <li class="list-item"><i class="fa fa-envelope fa-2x"><span class="contact-text gmail"><a :href="'mailto:' + contactInfo.email" >{{ contactInfo.email }}</a></span></i></li>
          </ul>
          <hr>
          <ul class="social-media-list">
            <li><a :href="contactInfo.linkedin" target="_blank" class="contact-icon"><i class="fa fa-linkedin" aria-hidden="true"></i></a></li>
            <li><a :href="contactInfo.instagram" target="_blank" class="contact-icon"><i class="fa fa-instagram" aria-hidden="true"></i></a></li>
          </ul>
          <hr>
          <div class="copyright">&copy; TODOS OS DIREITOS RESERVADOS</div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "SectionContact",
  data() {
    return {
      formData: {
        name: '',
        email: '',
        message: ''
      },
      contactInfo: {
        location: "Joinville, Santa Catarina",
        phone: "(212) 555-2368",
        email: "contato@iztoic.com",
        linkedin: "#",
        instagram: "https://www.instagram.com/iztoic"
      }
    };
  },
  methods: {
    submitForm() {
      this.formData.name = '';
      this.formData.email = '';
      this.formData.message = '';
      this.$emit('form-submitted');
    }
  }
}
</script>
    
<style scoped>
section {
  border: 3px solid #d7a449;
  border-right: 3px solid transparent;
  margin: 0;
  padding: 0;
  background-color: #1f2029;
  padding-bottom: 50px;
}

#section-contact {
  width: 100vw;
  height: 100vh;
}

.section-header {
  text-align: center;
  margin: 0 auto;
  padding: 40px 0;
  font: 300 60px 'Oswald', sans-serif;
  color: #d7a449;
  text-transform: uppercase;
  letter-spacing: 6px;
}

.contact-wrapper {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 0 auto;
  padding: 20px;
  position: relative;
  max-width: 840px;
}

/* Left contact page */
.form-horizontal {
  /*float: left;*/
  max-width: 400px;
  font-family: 'Lato';
  font-weight: 400;
}

.form-control,
textarea {
  max-width: 400px;
  background-color: #000;
  color: #d7a449;
  letter-spacing: 1px;
}

.send-button {
  margin-top: 15px;
  height: 34px;
  width: 400px;
  overflow: hidden;
  transition: all .2s ease-in-out;
}

.alt-send-button {
  width: 400px;
  height: 34px;
  transition: all .2s ease-in-out;
}

.send-text {
  display: block;
  margin-top: 10px;
  font: 700 12px 'Lato', sans-serif;
  letter-spacing: 2px;
}

.alt-send-button:hover {
  transform: translate3d(0px, -29px, 0px);
}

/* Begin Right Contact Page */
.direct-contact-container {
  max-width: 400px;
}

/* Location, Phone, Email Section */
.contact-list {
  list-style-type: none;
  margin-left: -30px;
  padding-right: 20px;
}

.list-item {
  line-height: 4;
  color: #d7a449;
}

.contact-text {
  font: 300 18px 'Lato', sans-serif;
  letter-spacing: 1.9px;
  color: #fff;
}

.place {
  margin-left: 62px;
}

.phone {
  margin-left: 56px;
}

.gmail {
  margin-left: 53px;
}

.contact-text a {
  color: #fff;
  text-decoration: none;
  transition-duration: 0.2s;
}

.contact-text a:hover {
  color: #d7a449;
  text-decoration: none;
}


/* Social Media Icons */
.social-media-list {
  position: relative;
  font-size: 22px;
  text-align: center;
  width: 100%;
  margin: 0 auto;
  padding: 0;
}

.social-media-list li a {
  color: #d7a449;
}

.social-media-list li {
  position: relative;
  display: inline-block;
  height: 60px;
  width: 60px;
  margin: 10px 3px;
  line-height: 60px;
  border-radius: 50%;
  color: #d7a449;
  background-color: rgb(27, 27, 27);
  cursor: pointer;
  transition: all .2s ease-in-out;
}

.social-media-list li:after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 60px;
  height: 60px;
  line-height: 60px;
  border-radius: 50%;
  opacity: 0;
  box-shadow: 0 0 0 1px #fff;
  transition: all .2s ease-in-out;
}

.social-media-list li:hover {
  background-color: #fff;
}

.social-media-list li:hover:after {
  opacity: 1;
  transform: scale(1.12);
  transition-timing-function: cubic-bezier(0.37, 0.74, 0.15, 1.65);
}

.social-media-list li:hover a {
  color: #000;
}

.copyright {
  font: 200 14px 'Oswald', sans-serif;
  color: #d7a449;
  letter-spacing: 1px;
  text-align: center;
}

hr {
  border-color: rgba(255, 255, 255, .6);
}

/* Begin Media Queries*/
@media screen and (max-width: 850px) {
  .contact-wrapper {
    display: flex;
    flex-direction: column;
  }

  .direct-contact-container,
  .form-horizontal {
    margin: 0 auto;
  }

  .direct-contact-container {
    margin-top: 60px;
    max-width: 300px;
  }

  .social-media-list li {
    height: 60px;
    width: 60px;
    line-height: 60px;
  }

  .social-media-list li:after {
    width: 60px;
    height: 60px;
    line-height: 60px;
  }
}

@media screen and (max-width: 569px) {

  .direct-contact-container,
  .form-wrapper {
    float: none;
    margin: 0 auto;
  }

  .form-control,
  textarea {

    margin: 0 auto;
  }


  .name,
  .email,
  textarea {
    width: 280px;
  }

  .direct-contact-container {
    margin-top: 60px;
    max-width: 280px;
  }

  .social-media-list {
    left: 0;
  }

  .social-media-list li {
    height: 55px;
    width: 55px;
    line-height: 55px;
    font-size: 2rem;
  }

  .social-media-list li:after {
    width: 55px;
    height: 55px;
    line-height: 55px;
  }

}

@media screen and (max-width: 410px) {
  .send-button {
    width: 99%;
  }
}
</style>