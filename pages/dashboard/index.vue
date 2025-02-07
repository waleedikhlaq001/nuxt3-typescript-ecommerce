<script setup lang="ts">
import { ref, computed } from "vue";
import DashboardPageHeader from "@/components/DashboardPageHeader.vue";

interface Order {
  id: number;
  status: string;
}

// Define orders data (Replace with real API data)
const orders = ref<Order[]>([
  { id: 1, status: "completed" },
  { id: 2, status: "pending" },
  { id: 3, status: "completed" },
  { id: 4, status: "canceled" },
  { id: 5, status: "pending" },
  { id: 6, status: "completed" },
]);

// Computed properties for order counts
const totalOrders = computed<number>(() => orders.value.length);
const completedOrders = computed<number>(() => orders.value.filter(order => order.status === "completed").length);
const pendingOrders = computed<number>(() => orders.value.filter(order => order.status === "pending").length);
const canceledOrders = computed<number>(() => orders.value.filter(order => order.status === "canceled").length);

definePageMeta({
  layout: "dashboard",
});

const logout = () => {};
</script>

<template>
  <div class="dashboard-home">
    <DashboardPageHeader title="Dashboard" />
    <div class="dashboard-home__content flex flex-wrap gap-4">
      <!-- Total Orders Card -->
      <div class="dashboard-card">
        <h3>Total Orders</h3>
        <p>{{ totalOrders }}</p>
      </div>

      <!-- Completed Orders Card -->
      <div class="dashboard-card">
        <h3>Completed Orders</h3>
        <p>{{ completedOrders }}</p>
      </div>

      <!-- Pending Orders Card -->
      <div class="dashboard-card">
        <h3>Pending Orders</h3>
        <p>{{ pendingOrders }}</p>
      </div>

      <!-- Canceled Orders Card -->
      <div class="dashboard-card">
        <h3>Canceled Orders</h3>
        <p>{{ canceledOrders }}</p>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.dashboard-home {
  &__content {
    display: flex;
    gap: 2rem;
  }
}

.dashboard-card {
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1);
  text-align: center;
  min-width: 200px;
  flex: 1;
}

.dashboard-card h3 {
  margin-bottom: 10px;
  font-size: 18px;
  color: #333;
}

.dashboard-card p {
  font-size: 24px;
  font-weight: bold;
	color: var(--accent-color);
}
</style>
