<script setup lang="ts">
import InputCompVue from "@/components/InputComp.vue";
import { ref } from "vue";

const test = ref();
</script>

<template>
  <h1>Hi</h1>
  <InputCompVue v-model="test" label="Test" />
</template>
