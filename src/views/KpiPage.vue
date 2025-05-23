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
        <ion-accordion value="1">
          <ion-item slot="header">
            <ion-label>1 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">1 Content</div>
        </ion-accordion>
        <ion-accordion value="2">
          <ion-item slot="header">
            <ion-label>2 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">2 Content</div>
        </ion-accordion>
        <ion-accordion value="3">
          <ion-item slot="header">
            <ion-label>3 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">3 Content</div>
        </ion-accordion>
        <ion-accordion value="4">
          <ion-item slot="header">
            <ion-label>4 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">4 Content</div>
        </ion-accordion>
        <ion-accordion value="5">
          <ion-item slot="header">
            <ion-label>5 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">5 Content</div>
        </ion-accordion>
      </ion-accordion-group>
      <br>
      <h1 class="ion-padding">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion value="1">
          <ion-item slot="header" >
            <ion-label>1 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">1 Content</div>
        </ion-accordion>
        <ion-accordion value="2">
          <ion-item slot="header">
            <ion-label>2 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">2 Content</div>
        </ion-accordion>
        <ion-accordion value="3">
          <ion-item slot="header">
            <ion-label>3 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">3 Content</div>
        </ion-accordion>
        <ion-accordion value="4">
          <ion-item slot="header">
            <ion-label>4 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">4 Content</div>
        </ion-accordion>
        <ion-accordion value="5">
          <ion-item slot="header">
            <ion-label>5 Accordion</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">5 Content</div>
        </ion-accordion>
      </ion-accordion-group>
    </ion-content>
  </ion-page>
</template>


<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonAccordionGroup, IonAccordion, IonItem, IonLabel } from '@ionic/vue';
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


</style>