<template>
   <div
      class="rates__item item"
      v-if="
         cur === currency.CharCode && this.currency_base !== currency.CharCode
      "
   >
      <div class="item__title">{{ currency.CharCode }}</div>
      <div class="item__value">
         {{
            (
               currency.Value / this.data.Valute[this.currency_base].Value
            ).toFixed(4)
         }}
      </div>
      <div
         class="item__progress"
         :class="progress_value(currency) > 0 ? 'green' : 'red'"
      >
         {{ progress_value(currency) }}
      </div>
      <button class="item__remove" @click="remove_currency(currency)">
         <p>Удалить валюту</p>
      </button>
   </div>
</template>

<script>
export default {
   props: {
      currency: {
         type: Object,
         required: true,
      },
      cur: {
         type: String,
         required: true,
      },
      currency_base: {
         type: String,
         required: true,
      },
      data: {
         type: Object,
         required: true,
      },
   },
   methods: {
      remove_currency(currency) {
         this.$emit("remove", currency);
      },
      progress_value(currency) {
         return (
            currency.Value / this.data.Valute[this.currency_base].Value -
            currency.Previous / this.data.Valute[this.currency_base].Previous
         ).toFixed(4);
      },
   },
};
</script>

<style lang="scss" scoped>
.rates {
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
   &__value {
      margin: 15px 0;
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
