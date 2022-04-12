<template>
   <div class="wrapper">
      <div class="container">
         <div class="converter">
            <h1>Конвертер валют</h1>
            <div class="converter__block">
               <div class="converter__from">
                  <select
                     v-model="currency_from"
                     name="currency-from"
                     class="converter__select select"
                     @change="conversion_change"
                  >
                     <option v-for="(item, i) in data.Valute" :key="i">
                        {{ item.CharCode }}
                     </option>
                  </select>
                  <input
                     type="number"
                     name="input-from"
                     class="converter__input-from"
                     v-model="value_from"
                     @input="conversion"
                  />
               </div>
               <div class="converter__to">
                  <select
                     v-model="currency_to"
                     name="currency-to"
                     class="converter__select select"
                     @change="conversion_change"
                  >
                     <option v-for="(item, i) in data.Valute" :key="i">
                        {{ item.CharCode }}
                     </option>
                  </select>
                  <input
                     type="number"
                     name="input-to"
                     class="converter__input-to"
                     disabled
                     placeholder="0"
                     v-model="value_to"
                  />
               </div>
            </div>
         </div>

         <div class="rates">
            <h1>Курсы валют</h1>
            <div class="rates__currency-base">
               <p class="rates__subtitle">Базовая валюта</p>
               <select
                  v-model="currency_base"
                  name="currency-base"
                  class="rates__select select"
               >
                  <option v-for="(item, i) in data.Valute" :key="i">
                     {{ item.CharCode }}
                  </option>
               </select>
            </div>
            <div class="rates__add-currency">
               <p class="rates__subtitle">Добавить валюту</p>
               <select
                  v-model="currency_add"
                  name="currency-add"
                  class="rates__select-add select"
               >
                  <option v-for="(item, i) in data.Valute" :key="i">
                     {{ item.CharCode }}
                  </option>
               </select>
               <button class="rates__add-btn" @click="add_currency">
                  <p>Добавить</p>
               </button>
            </div>
            <div class="rates__items">
               <div v-for="cur in currencies" :key="cur">
                  <div v-for="currency in data.Valute" :key="currency.CharCode">
                     <div
                        class="rates__item item"
                        v-if="
                           cur === currency.CharCode &&
                           this.currency_base !== currency.CharCode
                        "
                     >
                        <div class="item__title">{{ currency.CharCode }}</div>
                        <div class="item__value">
                           {{
                              (
                                 currency.Value /
                                 this.data.Valute[this.currency_base].Value
                              ).toFixed(4)
                           }}
                        </div>
                        <div
                           class="item__progress"
                           :class="
                              (
                                 currency.Value /
                                    this.data.Valute[this.currency_base].Value -
                                 currency.Previous /
                                    this.data.Valute[this.currency_base]
                                       .Previous
                              ).toFixed(4) > 0
                                 ? 'green'
                                 : 'red'
                           "
                        >
                           {{
                              (
                                 currency.Value /
                                    this.data.Valute[this.currency_base].Value -
                                 currency.Previous /
                                    this.data.Valute[this.currency_base]
                                       .Previous
                              ).toFixed(4)
                           }}
                        </div>
                        <button
                           class="item__remove"
                           @click="remove_currency(currency)"
                        >
                           <p>Удалить валюту</p>
                        </button>
                     </div>
                  </div>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>

<script>
export default {
   data() {
      return {
         data: [],
         currency_from: "USD",
         currency_to: "RUB",
         value_from: 1,
         value_to: 0,
         currencies: ["RUB", "USD", "EUR", "GBP"],
         currency_base: "RUB",
         base_value: 0,
         currency_add: "",
      };
   },
   methods: {
      fetchData() {
         fetch(`https://www.cbr-xml-daily.ru/daily_json.js`)
            .then((res) => res.json())
            .then((data) => {
               data.Valute.RUB = {
                  CharCode: "RUB",
                  ID: "Rub",
                  Name: "Российский рубль",
                  Nominal: 1,
                  NumCode: "643",
                  Previous: 1,
                  Value: 1,
               };
               this.data = data;
               this.calculationValueTwo();
            });
      },
      calculationValueTwo() {
         this.value_to = (
            (this.value_from * this.data.Valute[this.currency_from].Value) /
            this.data.Valute[this.currency_to].Value
         ).toFixed(4);
      },
      conversion() {
         this.calculationValueTwo();
      },
      conversion_change() {
         this.value_from = 1;
         this.calculationValueTwo();
      },
      add_currency() {
         if (!this.currencies.includes(this.currency_add)) {
            this.currencies.push(this.currency_add);
         }
      },
      remove_currency(currency) {
         this.currencies = this.currencies.filter(
            (e) => e !== currency.CharCode
         );
      },
   },
   mounted() {
      this.fetchData();
      setInterval(() => {
         this.fetchData();
      }, 60000);
   },
};
</script>

<style lang="scss">
@import "src/reset";
* {
   box-sizing: border-box;
}
h1 {
   font-size: 24px;
   color: rgb(0, 15, 100);
   margin: 15px 0;
   text-align: center;
   font-weight: 600;
}
.green {
   color: rgb(5, 133, 9);
}
.red {
   color: rgb(206, 0, 0);
}
.wrapper {
   display: flex;
   justify-content: center;
}

.container {
   width: 600px;
   border: 4px solid rgb(0, 15, 100);
   border-radius: 10px;
   background-color: rgb(241, 245, 255);
}

.select {
   padding: 3px;
   border-radius: 8px;
   background-color: rgb(204, 255, 214);
   cursor: pointer;
   transition: all 0.3s;
   &:hover {
      background-color: rgb(132, 255, 107);
   }
}

.converter {
   &__block {
      margin: 0 15px;
   }
   &__from {
      margin-bottom: 10px;
   }
   &__to {
   }
   &__input-from {
      padding: 3px;
      background-color: rgb(229, 255, 234);
      width: 160px;
   }
   &__input-to {
      padding: 3px;
      background-color: rgb(229, 255, 234);
      width: 160px;
   }
   &__select {
      margin-right: 15px;
   }
}

.rates {
   &__currency-base {
      display: flex;
      align-items: center;
      padding: 7px 10px;
   }
   &__subtitle {
      font-size: 18px;
      margin: 0 5px;
   }
   &__select {
      margin-left: 25px;
   }
   &__add-currency {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      padding: 7px 10px;
   }
   &__select-add {
      margin-left: 10px;
   }
   &__add-btn {
      padding: 3px 8px;
      margin-left: 15px;
      border: 1px solid;
      border-radius: 8px;
      background-color: rgb(204, 255, 214);
      position: relative;
      top: 0;
      transition: all 0.3s;
      &:hover {
         background-color: rgb(132, 255, 107);
      }
      &:active {
         top: 3px;
      }
   }
   &__items {
      margin: 10px 0;
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
   }
   &__item {
      display: flex;
      justify-content: center;
      flex-direction: column;
      margin: 10px 15px;
      border: 2px solid rgb(0, 15, 100);
      border-radius: 8px;
      text-align: center;
      font-size: 18px;
      padding: 10px;
      background-color: rgb(216, 226, 255);
      @media (max-width: 460px) {
         margin: 10px 10px;
      }
   }
}

.item {
   &__title {
   }
   &__value {
      margin: 15px 0;
   }
   &__progress {
   }
   &__remove {
      margin: 15px 0 5px;
      width: 90px;
      padding: 3px 8px;
      border: 1px solid;
      border-radius: 8px;
      background-color: rgb(204, 255, 214);
      position: relative;
      top: 0;
      transition: all 0.3s;
      &:hover {
         background-color: rgb(132, 255, 107);
      }
      &:active {
         top: 3px;
      }
   }
}
</style>
