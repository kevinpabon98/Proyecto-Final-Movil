<template>
  <ion-page>
    <ion-header>
      <ion-toolbar style="background: rgba(0, 0, 0, 0.5);">
        <ion-buttons slot="start">
          <ion-back-button default-href="/" />
        </ion-buttons>
        <ion-title style="text-align: center; color: white">Configuración</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <ion-list>
        <!-- Botones principales -->
        <ion-item button>
          <ion-label>Cambiar Idioma</ion-label>
        </ion-item>
        <ion-item button>
          <ion-label>Limpiar Recordatorios</ion-label>
        </ion-item>
        <ion-item button @click="showAboutModal">
          <ion-label>Acerca de la Aplicación</ion-label>
        </ion-item>
        <ion-item button @click="showSocialMediaModal">
          <ion-label>Mis Redes Sociales</ion-label>
        </ion-item>
        <ion-item button @click="showContactModal">
          <ion-label>Contacto</ion-label>
        </ion-item>
        <ion-item button @click="showRateModal">
          <ion-label>Calificar la App</ion-label>
        </ion-item>
      </ion-list>
    </ion-content>

    <!-- Modal de acerca de la aplicación -->
    <ion-modal :is-open="isAboutModalOpen" @did-dismiss="closeAboutModal">
      <ion-header>
        <ion-toolbar>
          <ion-title>Acerca de la Aplicación</ion-title>
          <ion-buttons slot="end">
            <ion-button @click="closeAboutModal">Cerrar</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <div class="about-container">
          <h3>Mi Aplicación</h3>
          <p>Esta aplicación te ayuda a gestionar tus recordatorios y tareas diarias de forma eficiente.</p>
        </div>
      </ion-content>
    </ion-modal>

    <!-- Modal de redes sociales -->
    <ion-modal :is-open="isSocialMediaModalOpen" @did-dismiss="closeSocialMediaModal">
      <ion-header>
        <ion-toolbar>
          <ion-title>Redes Sociales</ion-title>
          <ion-buttons slot="end">
            <ion-button @click="closeSocialMediaModal">Cerrar</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <div class="social-media-container">
          <a href="https://facebook.com" target="_blank" class="social-button" style="background-color: #3b5998;">
            <ion-icon name="logo-facebook" class="social-icon"></ion-icon>
            Facebook
          </a>
          <a href="https://instagram.com" target="_blank" class="social-button" style="background-color: #E1306C;">
            <ion-icon name="logo-instagram" class="social-icon"></ion-icon>
            Instagram
          </a>
          <a href="https://twitter.com" target="_blank" class="social-button" style="background-color: #1DA1F2;">
            <ion-icon name="logo-twitter" class="social-icon"></ion-icon>
            Twitter
          </a>
        </div>
      </ion-content>
    </ion-modal>

    <!-- Modal de contacto -->
    <ion-modal :is-open="isContactModalOpen" @did-dismiss="closeContactModal">
      <ion-header>
        <ion-toolbar>
          <ion-title>Contacto</ion-title>
          <ion-buttons slot="end">
            <ion-button @click="closeContactModal">Cerrar</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <div class="contact-container">
          <img src="@/assets/kevin.jpeg" alt="Kevin Pabon" class="contact-photo" />
          <h2 class="contact-name">Kevin Pabon</h2>
          <p class="contact-info">Teléfono: +57 3213167959</p>
          <img src="@/assets/yesid.jpeg" alt="Yesid Ortiz" class="contact-photo" />
          <h2 class="contact-name">Yesid Ortiz</h2>
          <p class="contact-info">Teléfono: +57 3122669443</p>
          <img src="@/assets/yesid.jpeg" alt="Yesid Ortiz" class="contact-photo" />
          <h2 class="contact-name">Cristian Florez</h2>
          <p class="contact-info">Teléfono: +57 3145139855</p>
        </div>
      </ion-content>
    </ion-modal>

    <!-- Modal de calificación con preguntas -->
    <ion-modal :is-open="isRateModalOpen" @did-dismiss="closeRateModal">
      <ion-header>
        <ion-toolbar>
          <ion-title>Calificanos</ion-title>
          <ion-buttons slot="end">
            <ion-button @click="closeRateModal">Cerrar</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <div class="rate-questions-container">
          <h3>¡Tu opinión es importante!</h3>
          <p>Selecciona un puntaje dependiendo de tu opinion respecto a la APP 1 siendo muy malo y 5 Excelente:</p>

          <!-- Lista de preguntas -->
          <ion-list>
            <ion-item v-for="(question, index) in questions" :key="index">
              <ion-label>{{ question }}</ion-label>
              <ion-select v-model="answers[index]" placeholder="Selecciona">
                <ion-select-option value="1">1 - Muy Mal</ion-select-option>
                <ion-select-option value="2">2 - Mal</ion-select-option>
                <ion-select-option value="3">3 - Regular</ion-select-option>
                <ion-select-option value="4">4 - Bien</ion-select-option>
                <ion-select-option value="5">5 - Excelente</ion-select-option>
              </ion-select>
            </ion-item>
          </ion-list>

          <!-- Botón de calificar -->
          <ion-button expand="block" @click="submitFeedback">Calificar</ion-button>
          <p v-if="feedbackSubmitted" class="thanks-message">¡Gracias por tu opinión!</p>
        </div>
      </ion-content>
    </ion-modal>
  </ion-page>
</template>

<script>
export default {
  data() {
    return {
      isAboutModalOpen: false,
      isSocialMediaModalOpen: false,
      isContactModalOpen: false,
      isRateModalOpen: false,
      feedbackSubmitted: false, // Indica si el feedback fue enviado
      questions: [
        "¿Que te Parece la Interfaz?",
        "¿Que te Parece la Navegación?",
        "¿Que te Parecen los Estilos?",
        "¿Que opinsa de la Aplicación?",
        "¿Qué tan satisfecho estás en general?"
      ],
      answers: Array(5).fill(null), // Almacena las respuestas de las 5 preguntas
    };
  },
  methods: {
    showAboutModal() {
      this.isAboutModalOpen = true;
    },
    closeAboutModal() {
      this.isAboutModalOpen = false;
    },
    showSocialMediaModal() {
      this.isSocialMediaModalOpen = true;
    },
    closeSocialMediaModal() {
      this.isSocialMediaModalOpen = false;
    },
    showContactModal() {
      this.isContactModalOpen = true;
    },
    closeContactModal() {
      this.isContactModalOpen = false;
    },
    showRateModal() {
      this.isRateModalOpen = true;
      this.feedbackSubmitted = false; // Restablece el estado
    },
    closeRateModal() {
      this.isRateModalOpen = false;
    },
    submitFeedback() {
      if (this.answers.some(answer => answer === null)) {
        alert("Por favor, responde todas las preguntas.");
        return;
      }
      this.feedbackSubmitted = true; // Muestra mensaje de agradecimiento
    },
  },
};
</script>

<style scoped>
/* Estilos generales */
ion-header {
  --background: #6200ea;
}
ion-title {
  color: white;
}

/* Modal de acerca de */
.about-container {
  text-align: center;
  margin-top: 20px;
}

/* Modal de redes sociales */
.social-media-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.social-button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 90%;
  max-width: 300px;
  height: 60px;
  border-radius: 30px;
  color: white;
  text-decoration: none;
}

.social-icon {
  margin-right: 10px;
  font-size: 1.8em;
}

/* Modal de contacto */
.contact-container {
  text-align: center;
}

.contact-photo {
  width: 150px;
  height: 150px;
  border-radius: 50%;
}

/* Modal de calificación con preguntas */
.rate-questions-container {
  text-align: center;
}

.thanks-message {
  margin-top: 15px;
  color: #4caf50;
  font-weight: bold;
}

ion-item {
  --background: transparent;
}

ion-label {
  font-size: 1.1em;
}
</style>
