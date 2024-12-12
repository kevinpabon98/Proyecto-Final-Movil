<template>
  <ion-page>
    <ion-header translucent>
      <ion-toolbar style="background: rgba(0, 0, 0, 0.5);">
        <ion-title style="text-align: center; color: white">Login</ion-title>
      </ion-toolbar>
    </ion-header>

    <!-- Contenido -->
    <ion-content fullscreen class="ion-padding-horizontal">
      <div id="login-container">
        <h1>Inicia Sesión</h1>
        <p>Introduce tus credenciales para continuar</p>

        <!-- Campo de correo electrónico -->
        <ion-input
          type="email"
          placeholder="Correo electrónico"
          class="input-field"
          @ionInput="updateEmail"
        ></ion-input>

        <!-- Campo de contraseña -->
        <ion-input
          type="password"
          placeholder="Contraseña"
          class="input-field"
          @ionInput="updatePassword"
        ></ion-input>

        <!-- Mensaje de error -->
        <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>

        <!-- Botón para iniciar sesión -->
        <ion-button expand="block" color="primary" href="taskvox">Iniciar Sesión</ion-button>

        <!-- Botón para registrarse -->
        <p>
          ¿No tienes cuenta?
          <ion-button fill="clear" size="small" href="/register">
            Regístrate aquí
          </ion-button>
        </p>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      errorMessage: "", // Mensaje de error
    };
  },
  methods: {
    ionViewWillEnter() {
      // Cambia dinámicamente el título
      document.title = "Login"; // Actualiza el título del navegador
      // Restablece el scroll al navegar
      document.documentElement.scrollTop = 0;
      document.body.scrollTop = 0;
    },
    updateEmail(event) {
      this.email = event.target.value.trim();
    },
    updatePassword(event) {
      this.password = event.target.value.trim(); 
    },
    login() {
      // Validación de campos
      console.log("Email:", this.email, "Password:", this.password);
      if (!this.email || !this.password) {
        this.errorMessage = "Todos los campos son obligatorios.";
        return;
      }

      // Validación del formato del correo electrónico
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailRegex.test(this.email)) {
        this.errorMessage = "Por favor, ingresa un correo electrónico válido.";
        return;
      }

      // Si todo está correcto, limpia el mensaje de error y redirige
      this.errorMessage = "";
      console.log("Usuario:", this.email, "Contraseña:", this.password);
      this.$router.push("/taskvox");
    },
    goToRegister() {
      this.$router.push({ path: "/register", force: true });
    },
  },
};
</script>

<style scoped>
ion-content {
  --background: url('/images/fondo.gif') no-repeat center center / cover;
  --padding-top: 0;
  --padding-bottom: 0;
  --padding-start: 0;
  --padding-end: 0;
}

/* Contenedor del formulario de login */
#login-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  max-width: 90%; 
  width: 400px;
  padding: 30px;
  background: rgba(12, 12, 12, 0.308);
  border: 2px solid #a3a3a3;
  border-radius: 15px;
  box-shadow: 0 0 15px #a3a3a3;
  color: white;
  text-align: center;
}

/* Título del contenedor */
#login-container h1 {
  font-size: 32px;
  margin-bottom: 10px;
  color: #ffffff;
  text-shadow: 0 0 10px #ffffff;
}

/* Texto informativo */
#login-container p {
  font-size: 18px;
  margin-bottom: 30px;
  color: #dcdcdc;
}

/* Campos de entrada */
.input-field {
  width: 100%;
  margin-bottom: 20px;
  padding: 15px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  border: 2px solid #a3a3a3;
  border-radius: 10px;
  box-shadow: 0 0 10px #a3a3a3;
}

/* Botón principal */
ion-button {
  --background: #00bcd4; /* Azul celeste */
  --color: white;
  --background-hover: #00a3c4;
  --background-activated: #0088a0; 
  border-radius: 10px;
  font-weight: bold;
}

/* Botón de registro */
ion-button[fill="clear"] {
  --color: black;
  font-weight: bold;
  text-decoration: underline;
}

/* Mensaje de error */
.error-message {
  color: #ff5252;
  font-size: 14px;
  margin-bottom: 15px;
  text-align: left;
}

/* Encabezado del formulario */
ion-toolbar {
  --background: rgba(0, 0, 0, 0.5); /* Fondo semitransparente */
}

ion-title {
  color: white;
  font-size: 24px;
  font-weight: bold;
}
</style>
