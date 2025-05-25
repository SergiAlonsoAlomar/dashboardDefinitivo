<template>
  <div class="gauge-container">
    <svg :width="size" :height="size" class="gauge-svg">
      <!-- Fondo del gauge -->
      <circle
        :cx="center"
        :cy="center"
        :r="radius"
        fill="none"
        :stroke="backgroundColor"
        :stroke-width="strokeWidth"
        class="gauge-background"
      />
      
      <!-- Progreso del gauge -->
      <circle
        :cx="center"
        :cy="center"
        :r="radius"
        fill="none"
        :stroke="progressColor"
        :stroke-width="strokeWidth"
        :stroke-dasharray="circumference"
        :stroke-dashoffset="strokeDashoffset"
        class="gauge-progress"
        stroke-linecap="round"
        transform="rotate(-90)"
        :transform-origin="`${center} ${center}`"
      />
      
      <!-- Texto central -->
      <text
        :x="center"
        :y="center - 5"
        text-anchor="middle"
        class="gauge-value"
        :font-size="fontSize"
        fill="#ffffff"
      >
        {{ formattedValue }}
      </text>
      
      <!-- Label -->
      <text
        :x="center"
        :y="center + 12"
        text-anchor="middle"
        class="gauge-label"
        :font-size="labelFontSize"
        fill="#8c8c8c"
      >
        {{ label }}
      </text>
      
      <!-- Indicador de objetivo -->
      <text
        :x="center"
        :y="center + 25"
        text-anchor="middle"
        class="gauge-target"
        :font-size="targetFontSize"
        fill="#10dc60"
      >
        Meta: {{ target.toLocaleString() }}
      </text>
    </svg>
    
    <!-- Porcentaje -->
    <div class="gauge-percentage">
      {{ percentage }}%
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  value: number;
  target: number;
  label: string;
  size?: number;
  strokeWidth?: number;
  backgroundColor?: string;
  progressColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  size: 120,
  strokeWidth: 6,
  backgroundColor: '#333333',
  progressColor: '#3880ff'
});

const center = computed(() => props.size / 2);
const radius = computed(() => (props.size - props.strokeWidth) / 2);
const circumference = computed(() => 2 * Math.PI * radius.value);
const percentage = computed(() => Math.round((props.value / props.target) * 100));
const strokeDashoffset = computed(() => {
  const progress = Math.min(props.value / props.target, 1);
  return circumference.value * (1 - progress);
});

const formattedValue = computed(() => {
  return props.value.toLocaleString();
});

const fontSize = computed(() => {
  return Math.max(10, props.size / 12);
});

const labelFontSize = computed(() => {
  return Math.max(7, props.size / 18);
});

const targetFontSize = computed(() => {
  return Math.max(6, props.size / 20);
});
</script>

<style scoped>
.gauge-container {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.gauge-svg {
  max-width: 100%;
  max-height: calc(100% - 15px);
}

.gauge-background {
  opacity: 0.3;
}

.gauge-progress {
  transition: stroke-dashoffset 0.5s ease-in-out;
}

.gauge-value {
  font-weight: bold;
}

.gauge-percentage {
  position: absolute;
  bottom: 0;
  font-size: 0.6rem;
  color: #3880ff;
  font-weight: bold;
}
</style>
