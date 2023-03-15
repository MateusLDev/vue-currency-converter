<template>
  <div class="currency-converter-result-area mt-6">
    <p class="currency-converter-resume">{{ currencyResume }}</p>

    <p class="currency-converter-result">
      {{ currencyResult }}
    </p>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  currencyResume: { type: String, default: "" },
  currencyResult: { type: String, default: "" },
});
</script>

<style>
.currency-converter-result-area {
  text-align: center;
}

.currency-converter-resume {
  color: #2f3542;
  font-size: 14px;
}

.currency-converter-result {
  color: #2e3647;
  font-size: 32px;
  font-weight: 700;

  margin-top: 20px;
}
</style>
