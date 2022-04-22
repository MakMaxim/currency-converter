<template>
   <div class="wrapper">
      <div class="container">
         <converter-page :data="data" />
         <rates-page
            :data="data"
            :currencies="currencies"
            @add="add_currency"
            @remove="remove_currency"
         />
      </div>
   </div>
</template>

<script>
import ConverterPage from "@/components/ConverterPage";
import RatesPage from "@/components/RatesPage";
export default {
   components: { ConverterPage, RatesPage },
   data() {
      return {
         data: [],
         currencies: ["RUB", "USD", "EUR", "GBP"],
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
               // this.calculationValueTwo();
            });
      },
      add_currency(currency_add) {
         if (!this.currencies.includes(currency_add)) {
            this.currencies.push(currency_add);
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
</style>
