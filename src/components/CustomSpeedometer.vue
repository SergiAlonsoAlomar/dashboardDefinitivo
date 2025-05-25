<template>
  <div class="speedometer-container">
    <svg :width="size" :height="size * 0.7" class="speedometer-svg">
      <!-- Arco de fondo -->
      <path
        :d="backgroundArc"
        fill="none"
        :stroke="backgroundColor"
        :stroke-width="strokeWidth"
        stroke-linecap="round"
      />
      
      <!-- Arco de progreso -->
      <path
        :d="progressArc"
        fill="none"
        :stroke="getProgressColor()"
        :stroke-width="strokeWidth"
        stroke-linecap="round"
      />
      
      <!-- Marcadores -->
      <g v-for="(marker, index) in markers" :key="index">
        <line
          :x1="marker.x1"
          :y1="marker.y1"
          :x2="marker.x2"
          :y2="marker.y2"
          stroke="#666"
          stroke-width="1"
        />
        <text
          :x="marker.textX"
          :y="marker.textY"
          text-anchor="middle"
          font-size="8"
          fill="#8c8c8c"
        >
          {{ marker.value }}
        </text>
      </g>
      
      <!-- Aguja -->
      <line
        :x1="center"
        :y1="center"
        :x2="needleEnd.x"
        :y2="needleEnd.y"
        stroke="#ffffff"
        stroke-width="2"
        stroke-linecap="round"
      />
      
      <!-- Centro de la aguja -->
      <circle
        :cx="center"
        :cy="center"
        r="4"
        fill="#ffffff"
      />
      
      <!-- Indicador de objetivo -->
      <line
        :x1="targetLine.x1"
        :y1="targetLine.y1"
        :x2="targetLine.x2"
        :y2="targetLine.y2"
        stroke="#10dc60"
        stroke-width="3"
        stroke-linecap="round"
      />
    </svg>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  value: number;
  max: number;
  target: number;
  size?: number;
  strokeWidth?: number;
  backgroundColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  size: 120,
  strokeWidth: 6,
  backgroundColor: '#333333'
});

const center = computed(() => props.size / 2);
const radius = computed(() => props.size / 2 - 15);
const startAngle = computed(() => -Math.PI * 0.75);
const endAngle = computed(() => Math.PI * 0.75);
const totalAngle = computed(() => endAngle.value - startAngle.value);

const backgroundArc = computed(() => {
  const start = polarToCartesian(center.value, center.value, radius.value, startAngle.value);
  const end = polarToCartesian(center.value, center.value, radius.value, endAngle.value);
  
  return `M ${start.x} ${start.y} A ${radius.value} ${radius.value} 0 1 1 ${end.x} ${end.y}`;
});

const progressArc = computed(() => {
  const progress = Math.min(props.value / props.max, 1);
  const progressAngle = startAngle.value + (totalAngle.value * progress);
  
  const start = polarToCartesian(center.value, center.value, radius.value, startAngle.value);
  const end = polarToCartesian(center.value, center.value, radius.value, progressAngle);
  
  const largeArcFlag = progress > 0.5 ? 1 : 0;
  
  return `M ${start.x} ${start.y} A ${radius.value} ${radius.value} 0 ${largeArcFlag} 1 ${end.x} ${end.y}`;
});

const needleEnd = computed(() => {
  const progress = Math.min(props.value / props.max, 1);
  const needleAngle = startAngle.value + (totalAngle.value * progress);
  return polarToCartesian(center.value, center.value, radius.value - 8, needleAngle);
});

const targetLine = computed(() => {
  const targetProgress = Math.min(props.target / props.max, 1);
  const targetAngle = startAngle.value + (totalAngle.value * targetProgress);
  
  const inner = polarToCartesian(center.value, center.value, radius.value - 10, targetAngle);
  const outer = polarToCartesian(center.value, center.value, radius.value + 3, targetAngle);
  
  return {
    x1: inner.x,
    y1: inner.y,
    x2: outer.x,
    y2: outer.y
  };
});

const markers = computed(() => {
  const markerCount = 4;
  const markers = [];
  
  for (let i = 0; i <= markerCount; i++) {
    const progress = i / markerCount;
    const angle = startAngle.value + (totalAngle.value * progress);
    const value = (props.max * progress).toFixed(1);
    
    const inner = polarToCartesian(center.value, center.value, radius.value - 3, angle);
    const outer = polarToCartesian(center.value, center.value, radius.value + 3, angle);
    const text = polarToCartesian(center.value, center.value, radius.value + 12, angle);
    
    markers.push({
      x1: inner.x,
      y1: inner.y,
      x2: outer.x,
      y2: outer.y,
      textX: text.x,
      textY: text.y,
      value
    });
  }
  
  return markers;
});

function polarToCartesian(centerX: number, centerY: number, radius: number, angleInRadians: number) {
  return {
    x: centerX + (radius * Math.cos(angleInRadians)),
    y: centerY + (radius * Math.sin(angleInRadians))
  };
}

function getProgressColor() {
  const progress = props.value / props.max;
  
  if (progress <= 0.4) return '#10dc60';
  if (progress <= 0.7) return '#ffce00';
  return '#f04141';
}
</script>

<style scoped>
.speedometer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.speedometer-svg {
  overflow: visible;
  max-width: 100%;
  max-height: 100%;
}
</style>
