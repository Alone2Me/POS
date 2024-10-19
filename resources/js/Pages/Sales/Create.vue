<script setup>
import { useLayout } from "@/Layouts/composables/layout";
import { ProductService } from "@/service/ProductService";
import { onMounted, ref, watch } from "vue";
import AppLayout from "@/Layouts/AppLayout.vue";

const { getPrimary, getSurface, isDarkTheme } = useLayout();

const products = ref(null);
const chartData = ref(null);
const chartOptions = ref(null);

const items = ref([
  { label: "Add New", icon: "pi pi-fw pi-plus" },
  { label: "Remove", icon: "pi pi-fw pi-trash" },
]);

onMounted(() => {
  ProductService.getProductsSmall().then((data) => (products.value = data));
  chartData.value = setChartData();
  chartOptions.value = setChartOptions();
});

function setChartData() {
  const documentStyle = getComputedStyle(document.documentElement);

  return {
    labels: ["Q1", "Q2", "Q3", "Q4"],
    datasets: [
      {
        type: "bar",
        label: "Subscriptions",
        backgroundColor: documentStyle.getPropertyValue("--p-primary-400"),
        data: [4000, 10000, 15000, 4000],
        barThickness: 32,
      },
      {
        type: "bar",
        label: "Advertising",
        backgroundColor: documentStyle.getPropertyValue("--p-primary-300"),
        data: [2100, 8400, 2400, 7500],
        barThickness: 32,
      },
      {
        type: "bar",
        label: "Affiliate",
        backgroundColor: documentStyle.getPropertyValue("--p-primary-200"),
        data: [4100, 5200, 3400, 7400],
        borderRadius: {
          topLeft: 8,
          topRight: 8,
        },
        borderSkipped: true,
        barThickness: 32,
      },
    ],
  };
}

function setChartOptions() {
  const documentStyle = getComputedStyle(document.documentElement);
  const borderColor = documentStyle.getPropertyValue("--surface-border");
  const textMutedColor = documentStyle.getPropertyValue("--text-color-secondary");

  return {
    maintainAspectRatio: false,
    aspectRatio: 0.8,
    scales: {
      x: {
        stacked: true,
        ticks: {
          color: textMutedColor,
        },
        grid: {
          color: "transparent",
          borderColor: "transparent",
        },
      },
      y: {
        stacked: true,
        ticks: {
          color: textMutedColor,
        },
        grid: {
          color: borderColor,
          borderColor: "transparent",
          drawTicks: false,
        },
      },
    },
  };
}

const formatCurrency = (value) => {
  return value.toLocaleString("en-US", { style: "currency", currency: "USD" });
};

watch([getPrimary, getSurface, isDarkTheme], () => {
  chartData.value = setChartData();
  chartOptions.value = setChartOptions();
});
</script>

<template>
  <app-layout>
    <div class="grid grid-cols-12 gap-4 mb-4">
      <div class="col-span-12 xl:col-span-8">
        <div class="card">
          <div class="font-semibold text-xl mb-4">All Product</div>
          <DataTable
            :value="products"
            :rows="10"
            :paginator="true"
            responsiveLayout="scroll"
          >
            <Column style="width: 15%" header="Image">
              <template #body="slotProps">
                <img
                  :src="`https://primefaces.org/cdn/primevue/images/product/${slotProps.data.image}`"
                  :alt="slotProps.data.image"
                  width="50"
                  class="shadow"
                />
              </template>
            </Column>
            <Column
              field="name"
              header="Name"
              :sortable="true"
              style="width: 35%"
            ></Column>
            <Column field="price" header="Price" :sortable="true" style="width: 35%">
              <template #body="slotProps">
                {{ formatCurrency(slotProps.data.price) }}
              </template>
            </Column>
            <Column style="width: 15%" header="View">
              <template #body>
                <Button icon="pi pi-search" type="button" class="p-button-text"></Button>
              </template>
            </Column>
          </DataTable>
        </div>
        
      </div>
      <div class="col-span-12 xl:col-span-4">
        <div class="card">
          <div class="font-semibold text-xl mb-4">All Product</div>
          <DataTable
            :value="products"
            :rows="10"
            :paginator="true"
            responsiveLayout="scroll"
          >
            <Column style="width: 15%" header="Image">
              <template #body="slotProps">
                <img
                  :src="`https://primefaces.org/cdn/primevue/images/product/${slotProps.data.image}`"
                  :alt="slotProps.data.image"
                  width="50"
                  class="shadow"
                />
              </template>
            </Column>
            <Column
              field="name"
              header="Name"
              :sortable="true"
              style="width: 35%"
            ></Column>
            <Column field="price" header="Price" :sortable="true" style="width: 35%">
              <template #body="slotProps">
                {{ formatCurrency(slotProps.data.price) }}
              </template>
            </Column>
            <Column style="width: 15%" header="View">
              <template #body>
                <Button icon="pi pi-search" type="button" class="p-button-text"></Button>
              </template>
            </Column>
          </DataTable>
        </div>
        
      </div>
    </div>

    <div class="grid grid-cols-12 gap-4">
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium  text-center mb-2">Sales</span>
              <div
                class="flex items-center justify-center bg-red-100 dark:bg-red-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-cart-plus text-red-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                $2.100
              </div> -->
            </div>
          </div>
        </div>
      </div>
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium mb-2">Drawer</span>
              <div
                class="flex items-center justify-center bg-green-100 dark:bg-green-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-sort-down text-green-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                $2.100
              </div> -->
            </div>
          </div>
        </div>
      </div>
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium mb-2">Customers</span>
              <div
                class="flex items-center justify-center bg-cyan-100 dark:bg-cyan-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-users text-cyan-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                28441
              </div> -->
            </div>
          </div>

        </div>
      </div>
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium mb-2">Revenue</span>
              <div
                class="flex items-center justify-center bg-pink-100 dark:bg-pink-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-dollar text-pink-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                $2.100
              </div> -->
            </div>
          </div>
        </div>
      </div>
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium mb-2">Discount</span>
              <div
                class="flex items-center justify-center bg-indigo-100 dark:bg-indigo-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-percentage text-indigo-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                $2.100
              </div> -->
            </div>
          </div>
        </div>
      </div>
      
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium mb-2">Sales List</span>
              <div
                class="flex items-center justify-center bg-cyan-100 dark:bg-cyan-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-table text-cyan-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                28441
              </div> -->
            </div>
          </div>

        </div>
      </div>
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium mb-2">Money Bill</span>
              <div
                class="flex items-center justify-center bg-orange-100 dark:bg-orange-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-money-bill text-orange-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                $2.100
              </div> -->
            </div>
          </div>
        </div>
      </div>
      <div class="col-span-12 lg:col-span-6 xl:col-span-1">
        <div class="card mb-0">
          <div class="flex justify-center text-center">
            <div>
              <span class="block text-muted-color font-medium mb-2">Invoices</span>
              <div
                class="flex items-center justify-center bg-purple-100 dark:bg-purple-400/10 rounded-border"
                style="width: 5.5rem; height: 4.5rem"
              >
                <i class="pi pi-print text-purple-500 !text-2xl"></i>
              </div>
              <!-- <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">
                $2.100
              </div> -->
            </div>
          </div>
        </div>
      </div>
      <div class="col-span-12 xl:col-span-4">
        <div class="card flex flex-col gap-0">
          <!-- <div class="font-semibold text-xl">Payment</div> -->
          <div class="grid grid-cols-12">
            <label
              for="name3"
              class="flex items-center col-span-12 mb-2 md:col-span-2 md:mb-0"
              >Grand Total</label
            >
            <div class="col-span-12 md:col-span-10">
              <Textarea
                rows="1"
                id="name3"
                type="text"
                class="w-full"
                value="1000.00$"
                disabled
              />
            </div>
          </div>
          <div class="grid grid-cols-12">
            <label
              for="email3"
              class="flex items-center col-span-12 mb-2 md:col-span-2 md:mb-0"
              >Discount</label
            >
            <div class="col-span-12 md:col-span-10">
              <Textarea
                rows="1"
                id="email3"
                type="text"
                class="w-full"
                value="10.00$"
                disabled=""
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </app-layout>
</template>
