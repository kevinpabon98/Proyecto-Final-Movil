<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar style="background: rgba(0, 0, 0, 0.5);">
        <ion-title style="text-align: center; color: white">Bienvenido :)</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large" style="text-align: center; color: white;">TaskVox</ion-title>
        </ion-toolbar>
      </ion-header>

      <div id="welcome-container">
        <h1>Bienvenido a TaskVox</h1>
        <p>Gestiona tus tareas y recordatorios de forma fácil y eficiente.</p>
        <ion-button expand="block" color="primary" href="/login">Comenzar</ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router';

// Instancia del router
const router = useRouter();

// Método para redirigir a la página de login
const goToLogin = () => {
  router.push({ name: 'Login' }); // Usar el nombre de la ruta para mayor precisión
};
</script>

<style scoped>
/* Fondo de la aplicación */
ion-content {
  --background: url('/images/fondo.gif') no-repeat center center / cover;
}

/* Contenedor de bienvenida */
#welcome-container {
  text-align: center;
  padding: 30px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(12, 12, 12, 0.308); /* Negro semitransparente */
  border: 2px solid #a3a3a3; /* Borde neón gris */
  border-radius: 15px;
  box-shadow: 0 0 15px #a3a3a3; /* Brillo alrededor */
  color: white;
  max-width: 80%;
  width: 600px;
}

/* Título del contenedor */
#welcome-container h1 {
  font-size: 32px;
  margin-bottom: 10px;
  color: #ffffff;
  text-shadow: 0 0 10px #ffffff; /* Efecto brillante */
}

/* Texto de bienvenida */
#welcome-container p {
  font-size: 18px;
  margin-bottom: 30px;
  color: #dcdcdc;
}

/* Estilo del botón */
ion-button {
  --background: #00bcd4; /* Azul celeste */
  --color: white;
  --background-hover: #00a3c4; /* Azul más oscuro al pasar el mouse */
  --background-activated: #0088a0; /* Azul más intenso al presionar */
  border-radius: 10px;
  font-weight: bold;
}

/* Estilo del encabezado */
ion-toolbar {
  --background: rgba(0, 0, 0, 0.5); /* Fondo semitransparente */
}

ion-title {
  color: white;
  font-size: 24px;
  font-weight: bold;
}
</style>
