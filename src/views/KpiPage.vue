<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🎯 KPIs</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🎯 KPIs</ion-title>
        </ion-toolbar>
      </ion-header>


      <h1 class="ion-padding">🚀 KPIs de Negocio</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in businessKPIs" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
      <br>
      <h1 class="ion-padding">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in technicalKPIs" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonAccordionGroup, IonAccordion, IonItem, IonLabel, IonList } from '@ionic/vue';
import { ref } from 'vue';

// Definición de la estructura de datos para un objetivo SMART
interface SmartElement {
  letter: string;
  content: string;
}

interface SmartGoal {
  id: number;
  title: string;
  description: string;
  smart: SmartElement[];
}

// Array de KPIs de Negocio (basados en datos actuales de los gráficos)
const businessKPIs = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Aumentar usuarios activos",
    description: "Incrementar el número de usuarios activos mensuales de 13,000 actuales a 17,000 (30% de crecimiento) en los próximos 6 meses mediante campañas de marketing digital y mejoras en la experiencia de usuario.",
    smart: [
      { letter: "S", content: "Incrementar usuarios activos mensuales" },
      { letter: "M", content: "de 13,000 a 17,000 usuarios (30% crecimiento)" },
      { letter: "A", content: "mediante campañas de marketing y mejoras UX" },
      { letter: "R", content: "para consolidar liderazgo en el mercado" },
      { letter: "T", content: "en los próximos 6 meses" }
    ]
  },
  {
    id: 2,
    title: "Mejorar tasa de predicciones",
    description: "Aumentar la tasa de predicciones del 60% actual al 75% de los usuarios activos (de 7,800 a 9,750 usuarios) para el próximo torneo importante mediante gamificación y recompensas por participación.",
    smart: [
      { letter: "S", content: "Aumentar tasa de participación en predicciones" },
      { letter: "M", content: "del 60% al 75% (de 7,800 a 9,750 usuarios)" },
      { letter: "A", content: "con gamificación y sistema de recompensas" },
      { letter: "R", content: "para aumentar engagement y retención" },
      { letter: "T", content: "para el próximo torneo importante" }
    ]
  },
  {
    id: 3,
    title: "Crecimiento de usuarios premium",
    description: "Incrementar los usuarios premium de 1,950 actuales (15%) a 2,600 usuarios (20%) en los próximos 8 meses ofreciendo análisis avanzados y contenido exclusivo de torneos.",
    smart: [
      { letter: "S", content: "Aumentar usuarios premium" },
      { letter: "M", content: "de 1,950 (15%) a 2,600 usuarios (20%)" },
      { letter: "A", content: "con análisis avanzados y contenido exclusivo" },
      { letter: "R", content: "para aumentar ingresos recurrentes" },
      { letter: "T", content: "en los próximos 8 meses" }
    ]
  },
  {
    id: 4,
    title: "Aumentar tiempo en app",
    description: "Incrementar el tiempo promedio de sesión de 8 minutos actuales a 12 minutos por usuario en los próximos 4 meses mediante contenido personalizado y funciones interactivas.",
    smart: [
      { letter: "S", content: "Aumentar tiempo promedio de sesión" },
      { letter: "M", content: "de 8 a 12 minutos por usuario" },
      { letter: "A", content: "con contenido personalizado y funciones interactivas" },
      { letter: "R", content: "para mejorar retención y engagement" },
      { letter: "T", content: "en los próximos 4 meses" }
    ]
  },
  {
    id: 5,
    title: "Expandir base de usuarios",
    description: "Crecer la base de usuarios registrados de 65,000 actuales a 85,000 (30% de crecimiento) en los próximos 10 meses mediante estrategias de referidos y partnerships con academias de pádel.",
    smart: [
      { letter: "S", content: "Expandir base de usuarios registrados" },
      { letter: "M", content: "de 65,000 a 85,000 usuarios (30% crecimiento)" },
      { letter: "A", content: "con referidos y partnerships con academias" },
      { letter: "R", content: "para posicionar la app como líder del sector" },
      { letter: "T", content: "en los próximos 10 meses" }
    ]
  }
]);

// Array de KPIs Técnicos (basados en datos actuales de los gráficos)
const technicalKPIs = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Reducir tiempo de carga",
    description: "Disminuir el tiempo de carga promedio de la app de 1.2s actuales a 0.8s en el 95% de los dispositivos para finales del próximo trimestre mediante optimización de código y CDN.",
    smart: [
      { letter: "S", content: "Reducir tiempo de carga de la app" },
      { letter: "M", content: "de 1.2s a 0.8s en 95% de dispositivos" },
      { letter: "A", content: "mediante optimización de código y CDN" },
      { letter: "R", content: "para mejorar experiencia de usuario" },
      { letter: "T", content: "para finales del próximo trimestre" }
    ]
  },
  {
    id: 2,
    title: "Mejorar tasa de retención",
    description: "Incrementar la retención de usuarios a 30 días del 40% actual al 55% en los próximos 6 meses implementando onboarding personalizado y notificaciones inteligentes.",
    smart: [
      { letter: "S", content: "Mejorar retención de usuarios a 30 días" },
      { letter: "M", content: "del 40% al 55%" },
      { letter: "A", content: "con onboarding personalizado y notificaciones inteligentes" },
      { letter: "R", content: "para aumentar valor de por vida del usuario" },
      { letter: "T", content: "en los próximos 6 meses" }
    ]
  },
  {
    id: 3,
    title: "Reducir tasa de errores",
    description: "Disminuir la tasa de errores críticos del 1% actual al 0.3% en los próximos 3 meses mediante testing automatizado y monitoreo proactivo.",
    smart: [
      { letter: "S", content: "Reducir tasa de errores críticos" },
      { letter: "M", content: "del 1% al 0.3%" },
      { letter: "A", content: "con testing automatizado y monitoreo proactivo" },
      { letter: "R", content: "para mejorar estabilidad y confianza" },
      { letter: "T", content: "en los próximos 3 meses" }
    ]
  },
  {
    id: 4,
    title: "Optimizar tiempo de respuesta",
    description: "Reducir el tiempo de respuesta promedio de la API de 28ms actuales a 15ms para el próximo gran torneo mediante optimización de base de datos y caché distribuido.",
    smart: [
      { letter: "S", content: "Optimizar tiempo de respuesta de API" },
      { letter: "M", content: "de 28ms a 15ms promedio" },
      { letter: "A", content: "con optimización de BD y caché distribuido" },
      { letter: "R", content: "para ofrecer experiencia más fluida" },
      { letter: "T", content: "para el próximo gran torneo" }
    ]
  },
  {
    id: 5,
    title: "Mejorar puntuación de stores",
    description: "Aumentar la puntuación promedio en App Store y Google Play de 4.5 estrellas actuales a 4.7 estrellas en los próximos 4 meses mediante mejoras UX y resolución proactiva de issues.",
    smart: [
      { letter: "S", content: "Mejorar puntuación en stores" },
      { letter: "M", content: "de 4.5 a 4.7 estrellas" },
      { letter: "A", content: "con mejoras UX y resolución proactiva" },
      { letter: "R", content: "para aumentar descargas orgánicas" },
      { letter: "T", content: "en los próximos 4 meses" }
    ]
  }
]);

// Combinamos ambos arrays para mostrar los mismos KPIs en ambas secciones (esto es temporal)
const smartGoals = ref<SmartGoal[]>([...businessKPIs.value, ...technicalKPIs.value]);
</script>

<style scoped>
ion-accordion.accordion-collapsing ion-item[slot='header'],
ion-accordion.accordion-collapsed ion-item[slot='header'] {
  --background: var(--ion-color-light);
  --color: var(--ion-color-light-contrast);
}

ion-accordion.accordion-expanding ion-item[slot='header'],
ion-accordion.accordion-expanded ion-item[slot='header'] {
  --background: rgba(var(--ion-color-primary-rgb), 0.14);
  color: var(--ion-color-primary);
}

/* Estilos para mantener la coherencia con tu app de pádel */
h1 {
  color: #C3FF00;
  font-weight: bold;
}

ion-content {
  --background: #1A1D0E;
}

.box {
  background: #2E2E34;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(195, 255, 0, 0.1);
  border: 1px solid #2E2E34;
}

/* Estilos para el tema oscuro */
@media (prefers-color-scheme: dark) {
  ion-content {
    --background: #1A1D0E;
  }
  
  .box {
    background: #2E2E34;
    color: #C3FF00;
  }
  
  h1 {
    color: #C3FF00;
  }
}
</style>