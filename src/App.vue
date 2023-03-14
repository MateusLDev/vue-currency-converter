<template>
  <div class="currency-wrapper" :class="{ active: currencyResult !== '' }">
    <h2 class="currency-title">Currency Converter</h2>

    <label for="amount-input">Amount</label>
    <v-text-field
      id="amount-input"
      v-model="currencyValue"
      variant="solo"
      @keypress="isNumber($event)"
    ></v-text-field>

    <div class="currency-converter-input-area">
      <div class="currency-converter-input mr-4">
        <label for="from-input">From</label>
        <v-select
          id="from-input"
          v-model="currencyBase"
          :items="listQuotes"
          variant="solo"
          hide-details
        ></v-select>
      </div>

      <img src="./assets/swap-horizontal.svg" width="30" class="mt-5" />

      <div class="currency-converter-input ml-4">
        <label for="to-input">To</label>
        <v-select
          id="to-input"
          v-model="currencyDestiny"
          :items="listQuotes"
          variant="solo"
          hide-details
        ></v-select>
      </div>
    </div>

    <v-btn
      block
      size="large"
      @click="converterCurrency"
      color="primary"
      class="mt-12 mb-4"
    >
      Convert
    </v-btn>

    <!-- <iframe src="https://embed.lottiefiles.com/animation/96438"></iframe> -->

    <div class="currency-converter-result-area mt-6">
      <p class="currency-converter-resume">{{ currencyResume }}</p>

      <p class="currency-converter-result">
        {{ currencyResult }}
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from "vue";
import axios from "axios";
import endpoint from "./endpoint";

const listQuotes = ref<string[]>([]);
const currencyResult = ref<string>("");
const currencyResume = ref<string>("");

const currencyValue = ref<number>(1);
const currencyBase = ref<string>("");
const currencyDestiny = ref<string>("");

const converterCurrency = async () => {
  const params = {
    from: currencyBase.value,
    to: currencyDestiny.value,
    q: currencyValue.value,
  };
  const { data } = await axios.get(endpoint.exchange, {
    params,
    headers: {
      "X-RapidAPI-Key": "a937728a14msh81ae1fd5abb1359p1772d5jsnc98ae0cecd84",
      "X-RapidAPI-Host": "currency-exchange.p.rapidapi.com",
    },
  });

  setCurrencyResume(data.toFixed(2));
  setCurrencyResult(data.toFixed(2));
};

const getListQuotes = async () => {
  try {
    const { data } = await axios.get(endpoint.quotes, {
      headers: {
        "X-RapidAPI-Key": "a937728a14msh81ae1fd5abb1359p1772d5jsnc98ae0cecd84",
        "X-RapidAPI-Host": "currency-exchange.p.rapidapi.com",
      },
    });

    listQuotes.value = data;
  } catch (error) {
    console.log("error");
  }
};

const setCurrencyResult = (currencyQuoted: number) => {
  currencyResult.value = `${(currencyValue.value * currencyQuoted).toFixed(
    2
  )} ${currencyDestiny.value}`;
};

const setCurrencyResume = (currencyQuoted: number) => {
  currencyResume.value = `1 ${currencyBase.value} = ${currencyQuoted} ${currencyDestiny.value}`;
};

const isNumber = (evt: KeyboardEvent) => {
  const keysAllowed: string[] = [
    "0",
    "1",
    "2",
    "3",
    "4",
    "5",
    "6",
    "7",
    "8",
    "9",
    ".",
  ];
  const keyPressed: string = evt.key;

  if (!keysAllowed.includes(keyPressed)) {
    evt.preventDefault();
  }
};

onMounted(async () => {
  getListQuotes();
});
</script>

<style scoped>
label {
  color: #2f3542;
}

.currency-wrapper {
  background-color: #fff;
  border-radius: 10px;

  width: 35rem;
  padding: 20px 30px;

  max-height: 404px;
  transition: max-height 0.3s;
}

.currency-wrapper.active {
  max-height: 493px;
}

.currency-title {
  color: #2f3542;
  margin-bottom: 30px;

  text-align: center;
}

.currency-converter-input-area {
  display: flex;
  justify-content: space-between;
}

.currency-converter-input {
  width: -webkit-fill-available;
}

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
