<template>
    <ion-page>
    <!-- Fondo persistente -->
    <div class="background"></div>
      <ion-header>
        <ion-toolbar style="background: rgba(0, 0, 0, 0.5);">
          <ion-buttons slot="start">
            <!-- Menú Lateral -->
            <ion-menu-button />
          </ion-buttons>
          <ion-title style="text-align: center; color: white">TaskVox</ion-title>
          <ion-buttons slot="end">
            <!-- Botón de Configuración con Imagen -->
            <ion-button href="configuracion">
              <img src="@/assets/confi.png" alt="Configuración" class="config-icon" />
            </ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
  
      <!-- Menú Lateral -->
      <ion-menu side="start" menu-id="first" content-id="main-content">
        <ion-header>
          <ion-toolbar>
            <ion-title>Menú</ion-title>
          </ion-toolbar>
        </ion-header>
        <ion-content>
          <ion-list>
            <ion-item button @click="goToHomePage">
              <ion-label>Inicio</ion-label>
            </ion-item>
            <ion-item button @click="goToRecordedTasks">
              <ion-label>Recordatorios Grabados</ion-label>
            </ion-item>
          </ion-list>
        </ion-content>
      </ion-menu>
  
      <!-- Página de Grabación -->
      <ion-content id="main-content" class="ion-padding" v-if="!showRecordedTasks">
        <div class="neon-box">
          <img src="@/assets/microfono1.png" alt="Micrófono" class="microphone-image" />
          <img src="@/assets/sonido.png" alt="Líneas de sonido" class="sound-lines-image" />
  
          <!-- Filtro por Categoría -->
          <div>
            <label for="category">Categoría:</label>
            <select v-model="selectedCategory">
              <option value="">Selecciona una categoría</option>
              <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
            </select>
          </div>
  
          <div class="button-group">
            <button @click="startRecording" class="icon-button">
              <img src="@/assets/grabar1.png" alt="Grabar" />
            </button>
            <button @click="playAudio" class="icon-button">
              <img src="@/assets/reproducir1.png" alt="Reproducir" />
            </button>
            <button @click="pauseAudio" class="icon-button">
              <img src="@/assets/pausa1.png" alt="Pausar" />
            </button>
            <button @click="stopAudio" class="icon-button">
              <img src="@/assets/stop1.png" alt="Detener" />
            </button>
          </div>
        </div>
      </ion-content>
  
      <!-- Página de Recordatorios Grabados -->
      <ion-content id="recorded-tasks" v-if="showRecordedTasks">
        <div class="task-list">
          <h3>Tareas Grabadas</h3>
  
          <!-- Filtro por categoría -->
          <div>
            <label for="filter-category">Filtrar por categoría:</label>
            <select v-model="selectedCategory">
              <option value="">Todas</option>
              <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
            </select>
          </div>
  
          <!-- Mostrar las tareas filtradas -->
          <ul>
            <li v-for="(task, index) in filteredTasks" :key="index">
              <p><strong>{{ task.date }}</strong>: {{ task.time }} (Categoría: {{ task.category }})</p>
              <button @click="playTaskAudio(task.audioUrl)">Reproducir</button>
            </li>
          </ul>
        </div>
      </ion-content>
    </ion-page>
  </template>
  
  
    <script>
    export default {
    name: "TaskVox",
    data() {
        return {
        mediaRecorder: null,
        audioChunks: [],
        audioBlob: null,
        audioUrl: null,
        audio: null,
        tasks: [], // Lista de tareas grabadas
        categories: ['Personal', 'Trabajo', 'Estudio'], // Categorías disponibles
        selectedCategory: '', // Categoría seleccionada para filtrar tareas
        showRecordedTasks: false, // Controlar la visualización de tareas grabadas
        };
    },
    computed: {
        filteredTasks() {
        if (this.selectedCategory) {
            return this.tasks.filter(task => task.category === this.selectedCategory);
        }
        return this.tasks;
        }
    },
    methods: {
        startRecording() {
        if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
            alert("Grabación no soportada en este navegador.");
            return;
        }

        navigator.mediaDevices
            .getUserMedia({ audio: true })
            .then((stream) => {
            this.mediaRecorder = new MediaRecorder(stream);
            this.audioChunks = [];

            this.mediaRecorder.ondataavailable = (event) => {
                this.audioChunks.push(event.data);
            };

            this.mediaRecorder.onstop = () => {
                this.audioBlob = new Blob(this.audioChunks, { type: "audio/mp3" });
                this.audioUrl = URL.createObjectURL(this.audioBlob);
                this.audio = new Audio(this.audioUrl);

                // Añadir la tarea grabada a la lista de tareas
                const now = new Date();
                const task = {
                date: now.toLocaleDateString(),
                time: now.toLocaleTimeString(),
                audioUrl: this.audioUrl,
                category: this.selectedCategory || 'Sin Categoría', // Usar categoría seleccionada
                };
                this.tasks.push(task);

                // Resetear categoría seleccionada después de grabar
                this.selectedCategory = '';

                // Cambio de vista al mostrar las tareas grabadas
                this.showRecordedTasks = true;
            };

            this.mediaRecorder.start();
            console.log("Grabación iniciada.");
            })
            .catch((error) => {
            console.error("Error al iniciar la grabación:", error);
            });
        },
        playAudio() {
        if (this.audio) {
            this.audio.play();
            console.log("Reproduciendo audio.");
        } else {
            alert("No hay audio grabado para reproducir.");
        }
        },
        pauseAudio() {
        if (this.audio) {
            this.audio.pause();
            console.log("Audio pausado.");
        }
        },
        stopAudio() {
        if (this.mediaRecorder && this.mediaRecorder.state !== "inactive") {
            this.mediaRecorder.stop();
            console.log("Grabación detenida.");
        }
        if (this.audio) {
            this.audio.pause();
            this.audio.currentTime = 0;
            console.log("Reproducción detenida.");
        }
        },
        playTaskAudio(audioUrl) {
        const audio = new Audio(audioUrl);
        audio.play();
        },
        goToHomePage() {
        this.showRecordedTasks = false; // Mostrar la página principal de grabación
        },
        goToRecordedTasks() {
        this.showRecordedTasks = true; // Mostrar las tareas grabadas
        },
        goToSettings() {
        this.$router.push("/configuracion"); // Redirigir a la página de configuración
        },
    },
    };
    </script>

  
  <style scoped>

ion-content {
  --background: url('/images/fondo.gif') no-repeat center center / cover;
}
  /* Fondo de la página */
  .background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1; /* Mantiene el fondo detrás del contenido */
    background-size: cover; /* Ajuste para cubrir toda la pantalla */
}
    /* Contenedor con borde de neón */
    .neon-box {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 30px; /* Aumentado para coincidir */
    border: 2px solid #a3a3a3; /* Borde gris similar */
    border-radius: 15px; /* Igual al otro */
    background: rgba(12, 12, 12, 0.308); /* Fondo negro semitransparente */
    position: absolute; /* Para centrarlo */
    top: 45%;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 15px #a3a3a3; /* Brillo gris alrededor */
    color: white; /* Color del texto */
    max-width: 80%; /* Ajuste de ancho */
    width: 600px; /* Igual al ejemplo */
    animation: neon-glow 1.5s ease-in-out infinite;
}
    .microphone-image {
    width: 100px;
    margin-bottom: 10px;
    }

    .sound-lines-image {
    width: 150px;
    margin-bottom: 20px;
    }

    .button-group {
    display: flex;
    gap: 15px;
    margin-top: 10px;
    }
    .icon-button {
    background: none;
    border: none;
    padding: 0;
    cursor: pointer;
    }

    .icon-button img {
    width: 70px;
    height: 70px;
    object-fit: contain;
    }

    /* Estilo de la lista de tareas grabadas */
    .task-list {
    margin-top: 20px;
    text-align: center;
    }

    .task-list h3 {
    color: black;
    }
    .config-icon{
        width: 45px;
        height: 45px;
    }

    ul {
    list-style-type: none;
    padding: 0;
    }

    li {
    padding: 10px;
    background-color: transparent;
    margin: 5px 0;
    border-radius: 5px;
    color: white;
    }

    select {
    padding: 5px;
    border-radius: 5px;
    margin-bottom: 10px;
    }

    button {
    background-color: #ff00c8;
    color: white;
    padding: 10px;
    border: none;
    cursor: pointer;
    border-radius: 5px;
    }
</style>

  