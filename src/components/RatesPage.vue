<template>
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
                              this.data.Valute[this.currency_base].Previous
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
                              this.data.Valute[this.currency_base].Previous
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
</template>

<script>
export default {
   props: {
      data: {
         type: Array,
         required: true,
      },
      currencies: {
         type: Array,
         required: true,
      },
   },
   data() {
      return {
         currency_base: "RUB",
         base_value: 0,
         currency_add: "",
      };
   },
   methods: {
      add_currency() {
         this.$emit("add", this.currency_add);
      },
      remove_currency(currency) {
         this.$emit("remove", currency);
      },
   },
};
</script>

<style lang="scss" scoped>
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
