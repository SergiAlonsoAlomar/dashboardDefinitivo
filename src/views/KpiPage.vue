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

// Array de KPIs de Negocio
const businessKPIs = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Aumentar usuarios activos",
    description: "Incrementar el número de usuarios activos mensuales en un 30% (de 10,000 a 13,000) en los próximos 6 meses mediante campañas de marketing digital y mejoras en la experiencia de usuario.",
    smart: [
      { letter: "S", content: "Incrementar usuarios activos mensuales" },
      { letter: "M", content: "30% (de 10,000 a 13,000)" },
      { letter: "A", content: "mediante campañas de marketing y mejoras UX" },
      { letter: "R", content: "para aumentar la base de usuarios fieles" },
      { letter: "T", content: "en los próximos 6 meses" }
    ]
  },
  {
    id: 2,
    title: "Mejorar tasa de predicciones",
    description: "Aumentar la participación en predicciones de partidos al 60% de los usuarios activos (de 4,000 a 7,800 usuarios) para el próximo torneo importante mediante notificaciones push y recompensas por participación.",
    smart: [
      { letter: "S", content: "Aumentar participación en predicciones" },
      { letter: "M", content: "60% de usuarios activos (4,000 a 7,800)" },
      { letter: "A", content: "con notificaciones push y sistema de recompensas" },
      { letter: "R", content: "para aumentar engagement con la app" },
      { letter: "T", content: "para el próximo torneo importante" }
    ]
  },
  {
    id: 3,
    title: "Crecimiento de usuarios premium",
    description: "Lograr que el 15% de los usuarios activos (1,950 usuarios) se conviertan en premium en los próximos 12 meses ofreciendo contenido exclusivo y estadísticas avanzadas.",
    smart: [
      { letter: "S", content: "Convertir usuarios en premium" },
      { letter: "M", content: "15% de usuarios activos (1,950)" },
      { letter: "A", content: "con contenido exclusivo y estadísticas avanzadas" },
      { letter: "R", content: "para aumentar ingresos recurrentes" },
      { letter: "T", content: "en los próximos 12 meses" }
    ]
  },
  {
    id: 4,
    title: "Aumentar tiempo en app",
    description: "Incrementar el tiempo promedio de sesión en la app de 5 a 8 minutos diarios por usuario en los próximos 3 meses mediante la implementación de contenido personalizado y notificaciones relevantes.",
    smart: [
      { letter: "S", content: "Aumentar tiempo promedio de sesión" },
      { letter: "M", content: "de 5 a 8 minutos diarios por usuario" },
      { letter: "A", content: "con contenido personalizado y notificaciones" },
      { letter: "R", content: "para mejorar retención de usuarios" },
      { letter: "T", content: "en los próximos 3 meses" }
    ]
  },
  {
    id: 5,
    title: "Expandir base de usuarios",
    description: "Aumentar la base de usuarios registrados en un 40% (de 50,000 a 70,000) en los próximos 9 meses mediante estrategias de referidos y colaboraciones con clubes de pádel.",
    smart: [
      { letter: "S", content: "Expandir base de usuarios registrados" },
      { letter: "M", content: "40% (de 50,000 a 70,000)" },
      { letter: "A", content: "con estrategias de referidos y colaboraciones" },
      { letter: "R", content: "para posicionar la app como líder en su sector" },
      { letter: "T", content: "en los próximos 9 meses" }
    ]
  }
]);

// Array de KPIs Técnicos
const technicalKPIs = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Reducir tiempo de carga",
    description: "Disminuir el tiempo de carga promedio de la app de 2.5s a 1.2s en el 90% de los dispositivos para finales del próximo trimestre mediante optimización de código y caché.",
    smart: [
      { letter: "S", content: "Reducir tiempo de carga de la app" },
      { letter: "M", content: "de 2.5s a 1.2s en 90% de dispositivos" },
      { letter: "A", content: "mediante optimización de código y caché" },
      { letter: "R", content: "para mejorar experiencia de usuario" },
      { letter: "T", content: "para finales del próximo trimestre" }
    ]
  },
  {
    id: 2,
    title: "Mejorar tasa de retención",
    description: "Incrementar la retención de usuarios a 30 días del 25% al 40% en los próximos 6 meses implementando un sistema de notificaciones personalizadas y recordatorios de predicciones.",
    smart: [
      { letter: "S", content: "Mejorar retención de usuarios a 30 días" },
      { letter: "M", content: "del 25% al 40%" },
      { letter: "A", content: "con notificaciones personalizadas y recordatorios" },
      { letter: "R", content: "para aumentar valor de por vida del usuario" },
      { letter: "T", content: "en los próximos 6 meses" }
    ]
  },
  {
    id: 3,
    title: "Reducir tasa de errores",
    description: "Disminuir la tasa de errores críticos reportados del 5% al 1% en los próximos 2 meses mediante pruebas exhaustivas y monitoreo continuo.",
    smart: [
      { letter: "S", content: "Reducir tasa de errores críticos" },
      { letter: "M", content: "del 5% al 1%" },
      { letter: "A", content: "con pruebas exhaustivas y monitoreo" },
      { letter: "R", content: "para mejorar estabilidad de la app" },
      { letter: "T", content: "en los próximos 2 meses" }
    ]
  },
  {
    id: 4,
    title: "Optimizar actualizaciones en vivo",
    description: "Lograr que el 95% de las actualizaciones en vivo de partidos tengan un retraso menor a 30 segundos para el próximo gran torneo mediante mejoras en la API y sistema de caché.",
    smart: [
      { letter: "S", content: "Optimizar actualizaciones en vivo" },
      { letter: "M", content: "95% con retraso <30 segundos" },
      { letter: "A", content: "con mejoras en API y sistema de caché" },
      { letter: "R", content: "para ofrecer información en tiempo real" },
      { letter: "T", content: "para el próximo gran torneo" }
    ]
  },
  {
    id: 5,
    title: "Mejorar puntuación de stores",
    description: "Aumentar la puntuación promedio en App Store y Google Play de 4.0 a 4.5 estrellas en los próximos 3 meses mediante corrección de bugs reportados y mejoras UX basadas en feedback.",
    smart: [
      { letter: "S", content: "Mejorar puntuación en stores" },
      { letter: "M", content: "de 4.0 a 4.5 estrellas" },
      { letter: "A", content: "corrigiendo bugs y mejorando UX" },
      { letter: "R", content: "para aumentar descargas orgánicas" },
      { letter: "T", content: "en los próximos 3 meses" }
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
  color: #3880ff; /* Color azul similar al de Ionic */
  font-weight: bold;
}

ion-content {
  --background: #f8f9fa; /* Fondo claro para contraste */
}

.box {
  background: #ffffff; /* Fondo blanco para las cajas */
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

/* Estilos para el tema oscuro si lo prefieres */
@media (prefers-color-scheme: dark) {
  ion-content {
    --background: #1e1e1e; /* Fondo oscuro */
  }
  
  .box {
    background: #2d2d2d; /* Fondo más oscuro para cajas */
    color: #ffffff;
  }
  
  h1 {
    color: #4c8dff; /* Azul más claro para modo oscuro */
  }
}
</style>