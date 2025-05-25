<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>📈 Técnico</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">📈 Técnico</ion-title>
        </ion-toolbar>
      </ion-header>

      <!-- Grid principal del Dashboard -->
      <ion-grid class="dashboard-grid">
        <!-- 🟢 Fila 1: 3 Columnas - KPIs técnicos -->
        <ion-row class="ion-row-1">
          <ion-col size="12" size-lg="4">
            <div class="box">
              <div class="kpi-card">
                <h3>Tiempo de Carga</h3>
                <div class="kpi-value">1.2s</div>
                <div class="kpi-improvement">-52% mejora</div>
              </div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box">
              <div class="kpi-card">
                <h3>Retención 30d</h3>
                <div class="kpi-value">40%</div>
                <div class="kpi-improvement">+15% mejora</div>
              </div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box">
              <div class="kpi-card">
                <h3>Tasa de Errores</h3>
                <div class="kpi-value">1%</div>
                <div class="kpi-improvement">-80% mejora</div>
              </div>
            </div>
          </ion-col>
        </ion-row>

        <!-- 🔵 Fila 2: 2 Columnas -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-md="3" push-md="9">
            <div class="box">
              <h4>Puntuación Stores</h4>
              <div ref="customGauge" class="custom-gauge"></div>
            </div>
          </ion-col>
          <ion-col size="12" size-md="9" pull-md="3">
            <div class="box">
              <h4>Rendimiento del Sistema</h4>
              <canvas ref="performanceChart" class="chart-canvas"></canvas>
            </div>
          </ion-col>
        </ion-row>

        <!-- 🟠 Fila 3: 3 Columnas -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <h4>Tiempo de Respuesta</h4>
              <div ref="responseTimeChart" class="chart-container"></div>
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <h4>Errores por Categoría</h4>
              <div ref="errorsChart" class="chart-container"></div>
            </div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">
              <h4>Monitoreo en Vivo</h4>
              <div ref="liveMonitoring" class="live-monitoring">
                <div class="status-indicator" :class="{ active: systemStatus }"></div>
                <div class="status-text">{{ systemStatus ? 'ONLINE' : 'OFFLINE' }}</div>
                <div class="metrics">
                  <div class="metric">
                    <span class="metric-label">CPU</span>
                    <span class="metric-value">{{ cpuUsage }}%</span>
                  </div>
                  <div class="metric">
                    <span class="metric-label">RAM</span>
                    <span class="metric-value">{{ ramUsage }}%</span>
                  </div>
                  <div class="metric">
                    <span class="metric-label">Latencia</span>
                    <span class="metric-value">{{ latency }}ms</span>
                  </div>
                </div>
                <canvas ref="liveChart" class="live-chart-canvas"></canvas>
              </div>
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonGrid, IonRow, IonCol } from '@ionic/vue';
import { ref, onMounted, onUnmounted } from 'vue';

// Referencias para los gráficos
const performanceChart = ref<HTMLCanvasElement>();
const customGauge = ref<HTMLDivElement>();
const responseTimeChart = ref<HTMLDivElement>();
const errorsChart = ref<HTMLDivElement>();
const liveMonitoring = ref<HTMLDivElement>();
const liveChart = ref<HTMLCanvasElement>();

// Estados reactivos para monitoreo en vivo
const systemStatus = ref(true);
const cpuUsage = ref(45);
const ramUsage = ref(62);
const latency = ref(28);

// Colores de la paleta
const colors = {
  primary: '#C3FF00',
  dark: '#1A1D0E',
  secondary: '#2E2E34'
};

let liveInterval: ReturnType<typeof setInterval>;
let chartData: number[] = [];

onMounted(async () => {
  await initCharts();
  startLiveMonitoring();
});

onUnmounted(() => {
  if (liveInterval) {
    clearInterval(liveInterval);
  }
});

const initCharts = async () => {
  // Chart.js - Gráfico de líneas múltiples para rendimiento
  await initPerformanceChart();
  
  // ApexCharts - Gráfico de área para tiempo de respuesta
  await initResponseTimeChart();
  
  // ECharts - Gráfico de barras para errores
  await initErrorsChart();
  
  // Gráfico propio - Gauge para puntuación de stores
  initCustomGauge();
};

const initPerformanceChart = async () => {
  if (!performanceChart.value) return;
  
  const { Chart, registerables } = await import('chart.js');
  Chart.register(...registerables);
  
  const ctx = performanceChart.value.getContext('2d');
  if (!ctx) return;
  
  new Chart(ctx, {
    type: 'line',
    data: {
      labels: ['00:00', '04:00', '08:00', '12:00', '16:00', '20:00'],
      datasets: [
        {
          label: 'Tiempo de Carga (s)',
          data: [2.5, 2.3, 2.0, 1.8, 1.5, 1.2],
          borderColor: colors.primary,
          backgroundColor: colors.primary + '30',
          tension: 0.4,
          yAxisID: 'y'
        },
        {
          label: 'Retención (%)',
          data: [25, 28, 32, 35, 38, 40],
          borderColor: '#FF6B6B',
          backgroundColor: '#FF6B6B30',
          tension: 0.4,
          yAxisID: 'y1'
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          labels: { color: colors.primary }
        }
      },
      scales: {
        x: { 
          ticks: { color: colors.primary },
          grid: { color: colors.secondary }
        },
        y: {
          type: 'linear',
          display: true,
          position: 'left',
          ticks: { color: colors.primary },
          grid: { color: colors.secondary }
        },
        y1: {
          type: 'linear',
          display: true,
          position: 'right',
          ticks: { color: '#FF6B6B' },
          grid: { drawOnChartArea: false }
        }
      }
    }
  });
};

const initResponseTimeChart = async () => {
  if (!responseTimeChart.value) return;
  
  const ApexCharts = (await import('apexcharts')).default;
  
  const options = {
    chart: {
      type: 'area',
      height: '100%',
      background: 'transparent',
      toolbar: { show: false }
    },
    series: [{
      name: 'Tiempo de Respuesta (ms)',
      data: [45, 38, 42, 35, 28, 25, 30, 28]
    }],
    xaxis: {
      categories: ['Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb', 'Dom', 'Hoy'],
      labels: { style: { colors: colors.primary } }
    },
    yaxis: {
      labels: { style: { colors: colors.primary } }
    },
    colors: [colors.primary],
    fill: {
      type: 'gradient',
      gradient: {
        shadeIntensity: 1,
        opacityFrom: 0.7,
        opacityTo: 0.1,
        stops: [0, 100]
      }
    },
    grid: {
      borderColor: colors.secondary
    },
    theme: {
      mode: 'dark'
    }
  };
  
  const chart = new ApexCharts(responseTimeChart.value, options);
  chart.render();
};

const initErrorsChart = async () => {
  if (!errorsChart.value) return;
  
  const echarts = await import('echarts');
  
  const chart = echarts.init(errorsChart.value, 'dark');
  
  const option = {
  backgroundColor: 'transparent',
  tooltip: {
    trigger: 'item',
    formatter: function(params: { seriesName: any; name: any; value: any; }) {
      return `<div style="color: ${colors.primary}; font-weight: bold;">${params.seriesName}</div>
              <div style="color: ${colors.primary};">${params.name}: ${params.value} errores</div>`;
    },
    backgroundColor: colors.dark,
    borderColor: colors.primary,
    textStyle: {
      color: colors.primary
    }
  },
  radar: {
    indicator: [
      { name: 'Auth', max: 20, color: colors.primary },
      { name: 'API', max: 20, color: colors.primary },
      { name: 'UI', max: 20, color: colors.primary },
      { name: 'DB', max: 20, color: colors.primary },
      { name: 'Red', max: 20, color: colors.primary }
    ],
    center: ['50%', '45%'],
    radius: '60%',
    axisName: {
      color: colors.primary,
      fontSize: 12,
      fontWeight: 'bold'
    },
    axisLine: {
      lineStyle: {
        color: colors.secondary,
        width: 1
      }
    },
    splitLine: {
      lineStyle: {
        color: colors.secondary,
        width: 1
      }
    },
    splitArea: {
      show: true,
      areaStyle: {
        color: [
          'rgba(195, 255, 0, 0.08)',
          'rgba(195, 255, 0, 0.04)'
        ]
      }
    },
    splitNumber: 4
  },
  series: [{
    name: 'Análisis de Errores',
    type: 'radar',
    data: [
      {
        value: [12, 8, 15, 5, 3],
        name: 'Errores Actuales',
        symbol: 'circle',
        symbolSize: 6,
        itemStyle: {
          color: colors.primary,
          borderColor: colors.primary,
          borderWidth: 2
        },
        areaStyle: {
          color: {
            type: 'radial',
            x: 0.5,
            y: 0.5,
            r: 0.5,
            colorStops: [
              { offset: 0, color: colors.primary + '30' },
              { offset: 1, color: colors.primary + '08' }
            ]
          }
        },
        lineStyle: {
          color: colors.primary,
          width: 3
        }
      },
      {
        value: [5, 3, 6, 2, 1],
        name: 'Objetivo Meta',
        symbol: 'diamond',
        symbolSize: 5,
        itemStyle: {
          color: '#FF6B6B',
          borderColor: '#FF6B6B',
          borderWidth: 2
        },
        areaStyle: {
          color: 'rgba(255, 107, 107, 0.15)'
        },
        lineStyle: {
          color: '#FF6B6B',
          width: 2,
          type: 'dashed'
        }
      }
    ]
  }],
  legend: {
    data: [
      {
        name: 'Errores Actuales',
        icon: 'circle',
        textStyle: {
          color: colors.primary,
          fontSize: 11,
          fontWeight: 'bold'
        }
      },
      {
        name: 'Objetivo Meta',
        icon: 'rect',
        textStyle: {
          color: '#FF6B6B',
          fontSize: 11,
          fontWeight: 'bold'
        }
      }
    ],
    bottom: 5,
    left: 'center',
    itemGap: 20,
    backgroundColor: 'rgba(26, 29, 14, 0.8)',
    borderColor: colors.secondary,
    borderWidth: 1,
    borderRadius: 4,
    padding: [8, 12]
  }
};
  
  chart.setOption(option);
  
  window.addEventListener('resize', () => chart.resize());
};

const initCustomGauge = () => {
  if (!customGauge.value) return;
  
  const container = customGauge.value;
  container.innerHTML = '';
  
  // Crear SVG para gauge
  const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
  svg.setAttribute('width', '100%');
  svg.setAttribute('height', '100%');
  svg.setAttribute('viewBox', '0 0 200 150');
  
  const centerX = 100;
  const centerY = 120;
  const radius = 60;
  const strokeWidth = 12;
  
  // Arco base
  const baseArc = document.createElementNS('http://www.w3.org/2000/svg', 'path');
  const startAngle = Math.PI;
  const endAngle = 2 * Math.PI;
  
  const x1 = centerX + radius * Math.cos(startAngle);
  const y1 = centerY + radius * Math.sin(startAngle);
  const x2 = centerX + radius * Math.cos(endAngle);
  const y2 = centerY + radius * Math.sin(endAngle);
  
  baseArc.setAttribute('d', `M ${x1} ${y1} A ${radius} ${radius} 0 0 1 ${x2} ${y2}`);
  baseArc.setAttribute('stroke', colors.secondary);
  baseArc.setAttribute('stroke-width', strokeWidth.toString());
  baseArc.setAttribute('fill', 'none');
  
  // Arco de progreso (4.5/5 = 90%)
  const progressArc = document.createElementNS('http://www.w3.org/2000/svg', 'path');
  const progressAngle = startAngle + (endAngle - startAngle) * 0.9;
  const px2 = centerX + radius * Math.cos(progressAngle);
  const py2 = centerY + radius * Math.sin(progressAngle);
  
  progressArc.setAttribute('d', `M ${x1} ${y1} A ${radius} ${radius} 0 0 1 ${px2} ${py2}`);
  progressArc.setAttribute('stroke', colors.primary);
  progressArc.setAttribute('stroke-width', strokeWidth.toString());
  progressArc.setAttribute('fill', 'none');
  progressArc.setAttribute('stroke-linecap', 'round');
  
  // Texto central
  const scoreText = document.createElementNS('http://www.w3.org/2000/svg', 'text');
  scoreText.setAttribute('x', centerX.toString());
  scoreText.setAttribute('y', (centerY - 10).toString());
  scoreText.setAttribute('text-anchor', 'middle');
  scoreText.setAttribute('fill', colors.primary);
  scoreText.setAttribute('font-size', '24');
  scoreText.setAttribute('font-weight', 'bold');
  scoreText.textContent = '4.5';
  
  const maxText = document.createElementNS('http://www.w3.org/2000/svg', 'text');
  maxText.setAttribute('x', centerX.toString());
  maxText.setAttribute('y', (centerY + 10).toString());
  maxText.setAttribute('text-anchor', 'middle');
  maxText.setAttribute('fill', colors.primary);
  maxText.setAttribute('font-size', '12');
  maxText.setAttribute('opacity', '0.7');
  maxText.textContent = '/ 5.0 ⭐';
  
  svg.appendChild(baseArc);
  svg.appendChild(progressArc);
  svg.appendChild(scoreText);
  svg.appendChild(maxText);
  container.appendChild(svg);
};

const startLiveMonitoring = () => {
  liveInterval = setInterval(() => {
    // Simular cambios en métricas
    cpuUsage.value = Math.max(20, Math.min(80, cpuUsage.value + (Math.random() - 0.5) * 10));
    ramUsage.value = Math.max(30, Math.min(90, ramUsage.value + (Math.random() - 0.5) * 8));
    latency.value = Math.max(15, Math.min(100, latency.value + (Math.random() - 0.5) * 15));
    
    // Ocasionalmente cambiar estado del sistema
    if (Math.random() < 0.05) {
      systemStatus.value = !systemStatus.value;
    }
    
    updateLiveChart();
  }, 1500);
};

const updateLiveChart = () => {
  if (!liveChart.value) return;
  
  const canvas = liveChart.value;
  const ctx = canvas.getContext('2d');
  if (!ctx) return;
  
  canvas.width = canvas.offsetWidth;
  canvas.height = 40;
  
  // Agregar nuevo punto de datos
  chartData.push(cpuUsage.value);
  if (chartData.length > 20) {
    chartData.shift();
  }
  
  // Limpiar canvas
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  
  // Dibujar línea de CPU
  ctx.strokeStyle = colors.primary;
  ctx.lineWidth = 2;
  ctx.beginPath();
  
  const stepX = canvas.width / (chartData.length - 1);
  
  chartData.forEach((value, index) => {
    const x = index * stepX;
    const y = canvas.height - (value / 100) * canvas.height;
    
    if (index === 0) {
      ctx.moveTo(x, y);
    } else {
      ctx.lineTo(x, y);
    }
  });
  
  ctx.stroke();
};
</script>

<style scoped>
ion-row {
  overflow: hidden;
}

ion-col {
  max-height: 100%;
  --ion-grid-column-padding: 10px;
}

.box {
  background: var(--ion-color-dark, #1A1D0E);
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  border: 1px solid #2E2E34;
}

.box h4 {
  color: #C3FF00;
  margin: 0 0 12px 0;
  font-size: 14px;
  font-weight: 600;
  flex-shrink: 0;
}

.kpi-card {
  text-align: center;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  flex: 1;
}

.kpi-card h3 {
  color: #C3FF00;
  font-size: 12px;
  margin: 0 0 8px 0;
  font-weight: 500;
}

.kpi-value {
  color: #C3FF00;
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 4px;
}

.kpi-improvement {
  color: #C3FF00;
  font-size: 14px;
  opacity: 0.8;
}

.chart-canvas {
  width: 100% !important;
  height: calc(100% - 30px) !important;
  max-height: calc(100% - 30px) !important;
  flex: 1;
}

.chart-container {
  height: calc(100% - 30px);
  max-height: calc(100% - 30px);
  width: 100%;
  flex: 1;
  overflow: hidden;
}

.custom-gauge {
  height: calc(100% - 30px);
  max-height: calc(100% - 30px);
  display: flex;
  align-items: center;
  justify-content: center;
  flex: 1;
  overflow: hidden;
}

.live-monitoring {
  height: 100%;
  max-height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  text-align: center;
  flex: 1;
  overflow: hidden;
}

.status-indicator {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #2E2E34;
  margin-bottom: 8px;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.status-indicator.active {
  background: #C3FF00;
  box-shadow: 0 0 10px #C3FF00;
}

.status-text {
  color: #C3FF00;
  font-size: 14px;
  font-weight: bold;
  margin-bottom: 12px;
  flex-shrink: 0;
}

.metrics {
  width: 100%;
  margin-bottom: 12px;
  flex-shrink: 0;
}

.metric {
  display: flex;
  justify-content: space-between;
  margin-bottom: 6px;
  font-size: 11px;
}

.metric-label {
  color: #C3FF00;
  opacity: 0.7;
}

.metric-value {
  color: #C3FF00;
  font-weight: bold;
}

.live-chart-canvas {
  width: 100%;
  height: 40px;
  max-height: 40px;
  flex-shrink: 0;
}

/* Aplicar altura total y por filas, solo en pantallas ≥ lg (992px) */
@media (min-width: 992px) {
  ion-grid { 
    height: 100%; 
    max-height: 100vh;
  }
  .ion-row-1 { 
    height: 20%; 
    max-height: 20%;
  }
  .ion-row-2 { 
    height: 40%; 
    max-height: 40%;
  }
  .ion-row-3 { 
    height: 40%; 
    max-height: 40%;
  }
}

/* Responsive para pantallas medianas (768px - 991px) */
@media (min-width: 768px) and (max-width: 991px) {
  ion-grid {
    height: auto;
  }
  
  ion-row {
    height: auto;
    max-height: none;
  }
  
  .box {
    height: 250px;
    max-height: 250px;
    margin-bottom: 16px;
  }
  
  .kpi-card {
    height: 100%;
  }
  
  .chart-container,
  .chart-canvas,
  .custom-gauge,
  .live-monitoring {
    height: calc(250px - 46px);
    max-height: calc(250px - 46px);
  }
}

/* Responsive para móviles (< 768px) */
@media (max-width: 767px) {
  ion-grid {
    height: auto;
  }
  
  ion-row {
    height: auto;
    max-height: none;
  }
  
  .box {
    height: 220px;
    max-height: 220px;
    margin-bottom: 16px;
  }
  
  .kpi-card {
    height: 100%;
  }
  
  .chart-container,
  .chart-canvas,
  .custom-gauge,
  .live-monitoring {
    height: calc(220px - 46px);
    max-height: calc(220px - 46px);
  }
  
  .kpi-value {
    font-size: 20px;
  }
  
  .status-text {
    font-size: 12px;
  }
  
  .metric {
    font-size: 10px;
  }
}

/* Asegurar que no hay overflow en ningún contenedor */
ion-content {
  overflow-x: hidden;
}

ion-grid {
  padding: 0;
  margin: 0;
  overflow: hidden;
}
</style>