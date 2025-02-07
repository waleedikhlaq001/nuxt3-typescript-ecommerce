<template>
  <div class="chart-container">
    <!-- Yearly Orders Bar Chart -->
    <div class="chart">
      <h2>Yearly Orders</h2>
      <apexchart height="400" width="100%" :options="barChartOptions" :series="barChartSeries"></apexchart>
    </div>

    <!-- Order Distribution Pie Chart -->
    <div class="chart">
      <h2>Order Distribution</h2>
      <apexchart height="400" width="100%" :options="pieChartOptions" :series="pieChartSeries"></apexchart>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

// 📊 Bar Chart (Yearly Orders)
const barChartOptions = ref({
  chart: {
    type: "bar",
    toolbar: { show: false },
  },
  colors: ["#3498db"], // Blue bars
  xaxis: {
    categories: [ "2020", "2021", "2022", "2023", "2024", "2025"],
    labels: { style: { fontSize: "14px", fontWeight: 500 } },
  },
  plotOptions: {
    bar: {
      borderRadius: 5,
      distributed: true,
    },
  },
});
const barChartSeries = ref([{ name: "Orders", data: [] }]);

// 🥧 Pie Chart (Order Status Distribution)
const pieChartOptions = ref({
  chart: { type: "pie" },
  labels: ["Total", "Completed", "Pending", "Canceled"],
  colors: ["#2ecc71", "#3498db", "#f1c40f", "#e74c3c"], // Green, Blue, Yellow, Red
});
const pieChartSeries = ref([]);

// 🔥 Sample Orders Data (Enhanced for Better Chart)
const orders = ref([
  { id: 9, status: "completed", year: "2020" },
  { id: 10, status: "canceled", year: "2020" },
  { id: 11, status: "completed", year: "2021" },
  { id: 12, status: "completed", year: "2022" },
  { id: 13, status: "pending", year: "2022" },
  { id: 14, status: "completed", year: "2023" },
  { id: 15, status: "canceled", year: "2023" },
  { id: 16, status: "pending", year: "2024" },
  { id: 17, status: "completed", year: "2024" },
  { id: 18, status: "canceled", year: "2025" },
  { id: 19, status: "completed", year: "2025" },
  { id: 20, status: "pending", year: "2025" },
  { id: 21, status: "completed", year: "2025" },
  { id: 22, status: "pending", year: "2021" },
  { id: 23, status: "completed", year: "2022" },
  { id: 24, status: "canceled", year: "2017" },
]);

const updateCharts = () => {
  // 📊 Yearly Orders (Bar Chart)
  const yearlyCounts: Record<string, number> = {
    "2020": 0, "2021": 0, "2022": 0, "2023": 0, "2024": 0, "2025": 0
  };

  orders.value.forEach(order => {
    if (yearlyCounts[order.year] !== undefined) {
      yearlyCounts[order.year]++;
    }
  });

  barChartSeries.value = [{ name: "Orders", data: Object.values(yearlyCounts) }];

  // 🥧 Order Status Distribution (Pie Chart)
  const total = orders.value.length;
  const completed = orders.value.filter(o => o.status === "completed").length;
  const pending = orders.value.filter(o => o.status === "pending").length;
  const canceled = orders.value.filter(o => o.status === "canceled").length;

  pieChartSeries.value = [total, completed, pending, canceled];
};

onMounted(() => {
  updateCharts();
});
</script>

<style scoped lang="scss">
.chart-container {
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px;
  padding: 20px;

  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr;
  }
}

.chart {
  background: #ffffff;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease-in-out;

  &:hover {
    transform: translateY(-5px);
  }

  h2 {
    text-align: center;
    margin-bottom: 15px;
    color: #333;
    font-size: 1.4rem;
    font-weight: bold;
  }
}
</style>
