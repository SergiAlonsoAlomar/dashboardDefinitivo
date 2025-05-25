<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🚀 Negocio</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🚀 Negocio</ion-title>
        </ion-toolbar>
      </ion-header>

      <!-- Grid principal del Dashboard -->
      <ion-grid class="dashboard-grid">
        <!-- 🟢 Fila 1: 4 Columnas - KPIs principales -->
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="kpi-card">
                <h3>Usuarios Activos</h3>
                <div class="kpi-value">13,000</div>
                <div class="kpi-growth">+30%</div>
              </div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="kpi-card">
                <h3>Tasa Predicciones</h3>
                <div class="kpi-value">60%</div>
                <div class="kpi-growth">+20%</div>
              </div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="kpi-card">
                <h3>Usuarios Premium</h3>
                <div class="kpi-value">1,950</div>
                <div class="kpi-growth">+15%</div>
              </div>
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <div class="kpi-card">
                <h3>Tiempo en App</h3>
                <div class="kpi-value">8 min</div>
                <div class="kpi-growth">+60%</div>
              </div>
            </div>
          </ion-col>
        </ion-row>

        <!-- 🔵 Fila 2: 2 Columnas -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="9">
            <div class="box">
              <h4>Crecimiento de Usuarios</h4>
              <canvas ref="chartjsCanvas" class="chart-canvas"></canvas>
            </div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">
              <h4>Distribución Premium</h4>
              <div ref="customChart" class="custom-chart"></div>
            </div>
          </ion-col>
        </ion-row>

        <!-- 🟠 Fila 3: 3 Columnas -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="4">
            <div class="box">
              <h4>Predicciones por Mes</h4>
              <div ref="apexChart" class="chart-container"></div>
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4">
            <div class="box">
              <h4>Tiempo de Sesión</h4>
              <div ref="echartsContainer" class="chart-container"></div>
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4">
            <div class="box">
              <h4>Usuarios en Tiempo Real</h4>
              <div ref="realtimeChart" class="realtime-chart">
                <div class="realtime-value">{{ realtimeUsers }}</div>
                <div class="realtime-label">Usuarios Activos</div>
                <canvas ref="realtimeCanvas" class="realtime-canvas"></canvas>
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
const chartjsCanvas = ref<HTMLCanvasElement>();
const customChart = ref<HTMLDivElement>();
const apexChart = ref<HTMLDivElement>();
const echartsContainer = ref<HTMLDivElement>();
const realtimeChart = ref<HTMLDivElement>();
const realtimeCanvas = ref<HTMLCanvasElement>();
const realtimeUsers = ref(1247);

// Colores de la paleta
const colors = {
  primary: '#C3FF00',
  dark: '#1A1D0E',
  secondary: '#2E2E34'
};

let realtimeInterval: ReturnType<typeof setInterval>;

onMounted(async () => {
  await initCharts();
  startRealtimeUpdates();
});

onUnmounted(() => {
  if (realtimeInterval) {
    clearInterval(realtimeInterval);
  }
});

const initCharts = async () => {
  // Chart.js - Gráfico de líneas para crecimiento de usuarios
  await initChartJS();
  
  // ApexCharts - Gráfico de barras para predicciones
  await initApexCharts();
  
  // ECharts - Gráfico de área para tiempo de sesión
  await initECharts();
  
  // Gráfico propio - Donut chart para distribución premium
  initCustomChart();
};

const initChartJS = async () => {
  if (!chartjsCanvas.value) return;
  
  // Importación dinámica de Chart.js
  const { Chart, registerables } = await import('chart.js');
  Chart.register(...registerables);
  
  const ctx = chartjsCanvas.value.getContext('2d');
  if (!ctx) return;
  
  new Chart(ctx, {
    type: 'line',
    data: {
      labels: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun'],
      datasets: [{
        label: 'Usuarios Activos',
        data: [10000, 10500, 11200, 11800, 12400, 13000],
        borderColor: colors.primary,
        backgroundColor: colors.primary + '20',
        tension: 0.4,
        fill: true
      }]
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
          ticks: { color: colors.primary },
          grid: { color: colors.secondary }
        }
      }
    }
  });
};

const initApexCharts = async () => {
  if (!apexChart.value) return;
  
  // Importación dinámica de ApexCharts
  const ApexCharts = (await import('apexcharts')).default;
  
  const options = {
    chart: {
      type: 'bar',
      height: '100%',
      background: 'transparent',
      toolbar: { show: false },
      offsetY: -10
    },
    plotOptions: {
      bar: {
        borderRadius: 4,
        dataLabels: {
          position: 'top'
        }
      }
    },
    series: [{
      name: 'Predicciones',
      data: [3200, 4100, 4800, 5500, 6800, 7800]
    }],
    xaxis: {
      categories: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun'],
      labels: { 
        style: { 
          colors: colors.primary,
          fontSize: '11px'
        },
        offsetY: 0
      },
      axisBorder: {
        show: true,
        color: colors.secondary
      },
      axisTicks: {
        show: true,
        color: colors.secondary
      }
    },
    yaxis: {
      labels: { 
        style: { 
          colors: colors.primary,
          fontSize: '11px'
        }
      }
    },
    colors: [colors.primary],
    grid: {
      borderColor: colors.secondary,
      padding: {
        bottom: 10
      }
    },
    theme: {
      mode: 'dark'
    },
    dataLabels: {
      enabled: false
    }
  };
  
  const chart = new ApexCharts(apexChart.value, options);
  chart.render();
};

const initECharts = async () => {
  if (!echartsContainer.value) return;
  
  const echarts = await import('echarts');
  
  const chart = echarts.init(echartsContainer.value, 'dark');
  
  const option = {
    backgroundColor: 'transparent',
    grid: {
      left: '10%',
      right: '10%',
      top: '15%',
      bottom: '15%',
      containLabel: true
    },
    tooltip: {
      trigger: 'axis',
      formatter: function(params: any[]) {
        return `<div style="color: ${colors.primary}; font-weight: bold;">Tiempo de Sesión</div>
                <div style="color: ${colors.primary};">${params[0].name}: ${params[0].value} minutos</div>`;
      },
      backgroundColor: colors.dark,
      borderColor: colors.primary,
      textStyle: {
        color: colors.primary
      }
    },
    xAxis: {
      type: 'category',
      data: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun'],
      axisLine: { 
        lineStyle: { 
          color: colors.primary,
          width: 2
        } 
      },
      axisTick: { 
        lineStyle: { 
          color: colors.primary,
          width: 1
        } 
      },
      axisLabel: { 
        color: colors.primary,
        fontSize: 12,
        fontWeight: 'bold'
      },
      splitLine: {
        show: false
      }
    },
    yAxis: {
      type: 'value',
      name: 'Minutos',
      nameTextStyle: {
        color: colors.primary,
        fontSize: 11
      },
      axisLine: { 
        lineStyle: { 
          color: colors.primary,
          width: 2
        } 
      },
      axisTick: { 
        lineStyle: { 
          color: colors.primary,
          width: 1
        } 
      },
      axisLabel: { 
        color: colors.primary,
        fontSize: 11
      },
      splitLine: { 
        lineStyle: { 
          color: colors.secondary,
          width: 1,
          type: 'dashed'
        } 
      },
      min: 0,
      max: 10
    },
    series: [{
      name: 'Tiempo de Sesión',
      data: [5.0, 5.2, 5.8, 6.5, 7.2, 8.0],
      type: 'line',
      smooth: true,
      symbol: 'circle',
      symbolSize: 8,
      areaStyle: {
        color: {
          type: 'linear',
          x: 0, y: 0, x2: 0, y2: 1,
          colorStops: [
            { offset: 0, color: colors.primary + '60' },
            { offset: 0.5, color: colors.primary + '30' },
            { offset: 1, color: colors.primary + '05' }
          ]
        }
      },
      lineStyle: { 
        color: colors.primary,
        width: 3
      },
      itemStyle: { 
        color: colors.primary,
        borderColor: colors.dark,
        borderWidth: 2
      },
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowColor: colors.primary
        }
      }
    }],
    animation: true,
    animationDuration: 1000
  };
  
  chart.setOption(option);
  
  // Redimensionar cuando cambie el tamaño
  const resizeHandler = () => {
    setTimeout(() => {
      chart.resize();
    }, 100);
  };
  
  window.addEventListener('resize', resizeHandler);
  
  // Cleanup en unmount
  onUnmounted(() => {
    window.removeEventListener('resize', resizeHandler);
    chart.dispose();
  });
};

const initCustomChart = () => {
  if (!customChart.value) return;
  
  const container = customChart.value;
  container.innerHTML = '';
  
  // Crear SVG para donut chart
  const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
  svg.setAttribute('width', '100%');
  svg.setAttribute('height', '100%');
  svg.setAttribute('viewBox', '0 0 200 200');
  
  // Datos para el gráfico
  const total = 13000;
  const premium = 1950;
  const regular = total - premium;
  
  const premiumPercentage = (premium / total) * 100;
  const regularPercentage = (regular / total) * 100;
  
  // Crear círculos del donut
  const centerX = 100;
  const centerY = 100;
  const radius = 70;
  const strokeWidth = 20;
  
  // Círculo base
  const baseCircle = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
  baseCircle.setAttribute('cx', centerX.toString());
  baseCircle.setAttribute('cy', centerY.toString());
  baseCircle.setAttribute('r', radius.toString());
  baseCircle.setAttribute('fill', 'none');
  baseCircle.setAttribute('stroke', colors.secondary);
  baseCircle.setAttribute('stroke-width', strokeWidth.toString());
  
  // Círculo premium
  const circumference = 2 * Math.PI * radius;
  const premiumStroke = (premiumPercentage / 100) * circumference;
  
  const premiumCircle = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
  premiumCircle.setAttribute('cx', centerX.toString());
  premiumCircle.setAttribute('cy', centerY.toString());
  premiumCircle.setAttribute('r', radius.toString());
  premiumCircle.setAttribute('fill', 'none');
  premiumCircle.setAttribute('stroke', colors.primary);
  premiumCircle.setAttribute('stroke-width', strokeWidth.toString());
  premiumCircle.setAttribute('stroke-dasharray', `${premiumStroke} ${circumference}`);
  premiumCircle.setAttribute('stroke-dashoffset', (circumference / 4).toString());
  premiumCircle.setAttribute('transform', `rotate(-90 ${centerX} ${centerY})`);
  
  // Texto central
  const text = document.createElementNS('http://www.w3.org/2000/svg', 'text');
  text.setAttribute('x', centerX.toString());
  text.setAttribute('y', centerY.toString());
  text.setAttribute('text-anchor', 'middle');
  text.setAttribute('dominant-baseline', 'middle');
  text.setAttribute('fill', colors.primary);
  text.setAttribute('font-size', '24');
  text.setAttribute('font-weight', 'bold');
  text.textContent = `${premiumPercentage.toFixed(1)}%`;
  
  svg.appendChild(baseCircle);
  svg.appendChild(premiumCircle);
  svg.appendChild(text);
  container.appendChild(svg);
  
  // Leyenda
  const legend = document.createElement('div');
  legend.className = 'chart-legend';
  legend.innerHTML = `
    <div class="legend-item">
      <span class="legend-color" style="background: ${colors.primary}"></span>
      Premium: ${premium.toLocaleString()}
    </div>
    <div class="legend-item">
      <span class="legend-color" style="background: ${colors.secondary}"></span>
      Regular: ${regular.toLocaleString()}
    </div>
  `;
  container.appendChild(legend);
};

const startRealtimeUpdates = () => {
  realtimeInterval = setInterval(() => {
    // Simular datos en tiempo real
    const variation = Math.floor(Math.random() * 20) - 10;
    realtimeUsers.value = Math.max(1000, realtimeUsers.value + variation);
    
    // Actualizar gráfico en tiempo real
    updateRealtimeChart();
  }, 2000);
};

const updateRealtimeChart = () => {
  if (!realtimeCanvas.value) return;
  
  const canvas = realtimeCanvas.value;
  const ctx = canvas.getContext('2d');
  if (!ctx) return;
  
  canvas.width = canvas.offsetWidth;
  canvas.height = 60;
  
  // Dibujar línea de pulso simple
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.strokeStyle = colors.primary;
  ctx.lineWidth = 2;
  
  const time = Date.now() / 1000;
  ctx.beginPath();
  
  for (let x = 0; x < canvas.width; x += 2) {
    const y = canvas.height / 2 + Math.sin((x + time * 50) * 0.02) * 15;
    if (x === 0) ctx.moveTo(x, y);
    else ctx.lineTo(x, y);
  }
  
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

.kpi-growth {
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

.custom-chart {
  height: calc(100% - 30px);
  max-height: calc(100% - 30px);
  display: flex;
  flex-direction: column;
  align-items: center;
  flex: 1;
  overflow: hidden;
}

.chart-legend {
  margin-top: 12px;
  font-size: 11px;
  flex-shrink: 0;
}

.legend-item {
  display: flex;
  align-items: center;
  margin-bottom: 4px;
  color: #C3FF00;
}

.legend-color {
  width: 12px;
  height: 12px;
  border-radius: 2px;
  margin-right: 6px;
}

.realtime-chart {
  text-align: center;
  height: 100%;
  max-height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  flex: 1;
  overflow: hidden;
}

.realtime-value {
  color: #C3FF00;
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 4px;
  flex-shrink: 0;
}

.realtime-label {
  color: #C3FF00;
  font-size: 12px;
  opacity: 0.8;
  margin-bottom: 12px;
  flex-shrink: 0;
}

.realtime-canvas {
  width: 100%;
  height: 60px;
  max-height: 60px;
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
    height: 45%; 
    max-height: 45%;
  }
  .ion-row-3 { 
    height: 35%; 
    max-height: 35%;
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
  .custom-chart,
  .realtime-chart {
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
  .custom-chart,
  .realtime-chart {
    height: calc(220px - 46px); /* 220px - padding - título */
    max-height: calc(220px - 46px);
  }
  
  .realtime-value {
    font-size: 24px;
  }
  
  .kpi-value {
    font-size: 20px;
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