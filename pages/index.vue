<template>
  <v-card outlined class="px-10 py-5 mx-auto my-10" max-width="600">
    <v-card-title>
      <span class="money material-icons"> paid </span>
    </v-card-title>

    <v-card-title class="mb-2 text-h4">Exchange Rate Calculator</v-card-title>
    <v-card-subtitle
      >Choose the currency and amount to get the exchange rate
    </v-card-subtitle>
    <v-card-text>
      <v-row dense class="mt-3">
        <v-col cols="6">
          <v-select
            :label="countryOne"
            v-model.trim="currencyOne"
            dense
            :items="currencyCodeList"
            outlined
            @change="calculate"
          ></v-select>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model.trim="amountOne"
            value="1"
            outlined
            type="number"
            dense
            @input="calculate"
          ></v-text-field>
        </v-col>
      </v-row>
      <div class="d-flex align-center justify-space-between">
        <v-btn depressed outlined @click="swapCurrency">
          <span class="material-icons"> swap_vert </span>
          <span>swap</span>
        </v-btn>
        <v-card-subtitle class="font-weight-bold">{{ rate }}</v-card-subtitle>
      </div>
      <v-row dense class="mt-5">
        <v-col cols="6">
          <v-select
            :label="countryTwo"
            v-model.trim="currencyTwo"
            dense
            :items="currencyCodeList"
            outlined
            @change="calculate"
          ></v-select>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model.trim="amountTwo"
            outlined
            type="number"
            dense
            @input="calculate"
          ></v-text-field>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
import currencyList from "@/api/currencyList";
export default {
  async fetch() {
    this.calculate();
    // console.log(this.currencyList);
  },
  data() {
    return {
      countryOne: "United State",
      countryTwo: "Bangladesh",
      currencyOne: "USD",
      currencyTwo: "BDT",
      amountOne: "1",
      amountTwo: "",
      currencyCodeList: [],
      currencyList: currencyList,
      rate: `1 BDT = 0.01 USD`,
    };
  },
  methods: {
    async calculate() {
      this.matchCurrencyOneFullName();
      this.matchCurrencyTwoFullName();
      const res = await fetch(
        `https://v6.exchangerate-api.com/v6/4d26051bb771b630d7a39a00/latest/${this.currencyOne}`
      );
      const data = await res.json();
      this.selectedMoney = data;
      let tempRate = data.conversion_rates[this.currencyTwo];
      this.rate = `1 ${this.currencyOne} = ${+tempRate.toFixed(2)} ${
        this.currencyTwo
      }`;
      this.amountTwo = (this.amountOne * tempRate).toFixed(2);
    },
    swapCurrency() {
      const tempCurrency = this.currencyOne;
      this.currencyOne = this.currencyTwo;
      this.currencyTwo = tempCurrency;
      this.calculate();
    },
    matchCurrencyOneFullName() {
      const currencyOneFullName = this.currencyList.filter((item) => {
        return item.code == this.currencyOne;
      });
      currencyOneFullName.forEach((item) => {
        this.countryOne = item.name;
      });
    },
    matchCurrencyTwoFullName() {
      const currencyTwoFullName = this.currencyList.filter((item) => {
        return item.code == this.currencyTwo;
      });
      currencyTwoFullName.forEach((item) => {
        this.countryTwo = item.name;
      });
    },
  },
  mounted() {
    const currencyCodeList = this.currencyList.map((item) => {
      return item.code;
    });
    this.currencyCodeList = currencyCodeList;
    this.matchCurrencyOneFullName();
    this.matchCurrencyTwoFullName();
  },
};
</script>

<style>
.money {
  font-size: 100px;
  margin-inline: auto;
}
</style>
