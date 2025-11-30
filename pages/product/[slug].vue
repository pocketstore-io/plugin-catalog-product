<template>
  <section class="grid grid-cols-6 gap-3 bg-gray-400 px-3 py-3">
    <div class="col-span-6 md:col-span-2">
      <img :src="getMediaUrl(item,'cover')" alt="">
      <ProductGallery :product="item"/>
    </div>
    <div class="col-span-6 md:col-span-4">
      <h2 class="font-bold text-lg">{{ item.name }}</h2>
      <p class="text-sm">{{ item.description }}</p>
      <CustomFields v-if="item.config" :fields="item.config"/>
      <section class="flex justify-between">
        <ProductStock :quantity="stock.quantity"/>
        <ProductAddToCart :product="item" :qty="stock.qty" :stock="stock"/>
      </section>

    </div>
    <div class="col-span-6">
      <ProductDetails :identifier="item.id"/>
      <ProductTabs :identifier="item.id"/>
    </div>
  </section>
</template>

<script lang="ts" setup>
import {usePocketBase} from "~/utils/pocketbase";
import ProductTabs from "~/components/catalog/ProductTabs.vue";
import CustomFields from "~/components/product/CustomFields.vue";
import ProductDetails from "~/components/catalog/ProductDetails.vue";

const route = useRoute();
const pb = usePocketBase();
const item = ref({});
const stock = ref({});

const load = async () => {
  item.value = await pb
      .collection("products")
      .getFirstListItem(
          'slug="' + route.params.slug.replace(".html", "") + '"',
      );

  useSeoMeta({
    title: item.value.name + " - Produkt Ansicht",
    description: item.value.description
  });
};

watch(() => item.value.id, async () => {
  stock.value = (
      await pb.collection("product_stocks").getList(1, 1, {
        filter: 'product="' + item.value.id + '"',
      })
  ).items[0];
});

onMounted(() => {
  load();
});
</script>