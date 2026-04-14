<script setup>
import { useRouter, useRoute } from "vue-router";
import { watch } from "vue";

const requiredHashValue = "#a=A+%26+B&b=123";

const router = useRouter();
const route = useRoute();

watch(
  () => route.hash,
  () => {
    if (route.hash) {
      if (route.hash != requiredHashValue) {
        console.error("`hash` value has changed!");
      } else {
        console.log("All good!");
      }
    } else {
      console.log("Hash is not set. Setting it now...");
      router.replace({ name: route.name, hash: requiredHashValue });
    }
  },
  { immediate: true },
);
</script>

<template>
  <div>
    <h2>HomeView</h2>
    <div v-if="route.hash" :class="{ error: route.hash != requiredHashValue }">
      <div v-if="route.hash == requiredHashValue" style="color: green">
        All good!
      </div>
      <div
        v-if="route.hash != requiredHashValue"
        style="color: red; background-color: #fcc"
      >
        Ups! hash value changed!
      </div>
      Expected Hash: {{ requiredHashValue }}<br />
      Actual Hash: {{ route.hash }}
    </div>
    <div v-else>Hash is not set.</div>
  </div>
</template>

<style>
.error {
  color: red;
}
</style>
