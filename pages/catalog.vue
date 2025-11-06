<template>
  <section class="page bg-white px-3 py-3">
    <section class="grid grid-cols-8 gap-3">
      <div class="col-span-4 md:col-span-2">
        Filter (Name)
      </div>
      <div class="col-span-4 md:col-span-2">
        Filter (Vendor)
      </div>
      <div class="col-span-4 md:col-span-2">
        Filter (Category)
      </div>
      <div class="col-span-4 md:col-span-2">
        Filter (Price) €
      </div>
      <div class="col-span-8">
        <section class="grid grid-cols-6 gap-3">
          <CatalogProductGrid identifier="bowling" />
        </section>
      </div>
    </section>
  </section>
</template>
<script setup lang="ts">
import CatalogProductGrid from '@/components/catalog/ProductGrid.vue'
</script>