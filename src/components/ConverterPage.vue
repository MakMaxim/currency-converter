<template>
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
</template>

<script>
export default {
   props: {
      data: {
         type: Array,
         required: true,
      },
   },
   data() {
      return {
         currency_from: "USD",
         currency_to: "RUB",
         value_from: 0,
         value_to: 0,
      };
   },
   methods: {
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
   },
};
</script>

<style lang="scss" scoped>
.converter {
   &__block {
      margin: 0 15px;
   }
   &__from {
      margin-bottom: 10px;
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
</style>
