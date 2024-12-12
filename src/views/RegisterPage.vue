<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar style="background: rgba(0, 0, 0, 0.5);">
        <ion-title style="text-align: center; color: white">Registro</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <!-- Fondo persistente -->
      <div class="background"></div>

      <!-- Contenedor de registro -->
      <div class="neon-box">
        <h2>Crea tu cuenta</h2>
        <!-- Formulario de registro -->
        <ion-input
          v-model="name"
          type="text"
          placeholder="Nombre completo"
          class="input-field"
        />
        <ion-input
          v-model="email"
          type="email"
          placeholder="Correo electrónico"
          class="input-field"
        />
        <ion-input
          v-model="password"
          type="password"
          placeholder="Contraseña"
          class="input-field"
        />
        <ion-button expand="full" href="login">Registrarse</ion-button>

        <p>
          ¿Ya tienes cuenta?
          <ion-button fill="clear" size="small" href="/login">
            Iniciar sesión
          </ion-button>
        </p>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
export default {
  name: "Register",
  data() {
    return {
      name: "",
      email: "",
      password: "",
    };
  },
  methods: {
    register() {
      console.log("Redirigiendo al login...");
      this.$router.push("/login"); // Redirige al login
    },
  },
};
</script>

<style scoped>
/* Fondo animado */
.background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* Mantener detrás del contenido */
  background: url('@/assets/fondo.gif') no-repeat center center fixed;
  background-size: cover;
}

/* Contenedor estilo Login */
.neon-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 30px;
  border: 2px solid #a3a3a3;
  border-radius: 15px;
  background: rgba(12, 12, 12, 0.308);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 15px #a3a3a3;
  color: white;
  max-width: 80%;
  width: 400px; /* Coincide con Login.vue */
}

/* Campos de entrada */
.input-field {
  margin-bottom: 20px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  border-radius: 10px;
  padding: 15px;
}

/* Botón con gradiente */
ion-button {
  background: linear-gradient(135deg, #ff7e5f, #feb47b);
  border-radius: 50px;
}

/* Enlace al login */
p {
  margin-top: 20px;
  color: #fff;
}
ion-title {
  color: white;
  font-size: 24px;
  font-weight: bold;
}

ion-router-link {
  color: #feb47b;
  text-decoration: none;
}
</style>
