<template>
    <ul class="flex-container">
        <li class="flex-item" id="total-sales"> 
            <h3> Ventas totales </h3>
            <h1> {{ sales.length }} </h1>
            <h3> {{ allTimeCash}} </h3>
        </li>
        <li class="flex-item" id="monthly-sales"> 
            <h3> Este Mes </h3>
            <h1> {{ thisMonth }} </h1>
            <h3> {{ allMonthCash}} </h3>
        </li>
        <li class="flex-item" id="weekly-sales"> 
            <h3> Semanal </h3>
            <h1> {{ thisWeek }} </h1>
            <h3> {{ allWeekCash}} </h3>
        </li>
        <li class="big">
            <h3> Principales Ventas </h3> 
            <ul id="top-products">
              <h5 v-for="(value, key) in topProducts" :key=key>
                {{ key+1 }}.- {{ value.category }} : {{ value.quantity}}
              </h5>
            </ul>
        </li>

        <b-tooltip target="total-sales"> Credito: {{creditTotal}} <br> Efectivo: {{cashTotal}} <br> Otro: {{4360 - creditTotal - cashTotal}}</b-tooltip>
        <b-tooltip target="monthly-sales"> Credito: {{monthCreditTotal}} <br> Efectivo: {{monthCashTotal}} <br> Otro: {{thisMonth - monthCreditTotal - monthCashTotal}}</b-tooltip>
        <b-tooltip target="weekly-sales"> Credito: {{weekCreditTotal}} <br> Efectivo: {{weekCashTotal}} <br> Otro: {{thisWeek - weekCreditTotal - weekCashTotal}}</b-tooltip>
    </ul>
</template>

<script>
export default {
    name: 'StatisticsPanel',
    props:
    {
        sales: {
            type: Array
            
        }
    },
    data() {
       return {
            thisWeek: 0,
            thisMonth: 0
        }
    },
    mounted() {
        this.thisMonth = this.sales.filter(sale => {
            return sale.date_closed.includes("2019-03")
        }).length
        this.thisWeek = this.sales.filter(sale => {
            return sale.date_closed.includes("2019-03-2")
        }).length
    },
      computed: {
    creditTotal: function () {
      return this.sales.filter(sale => {
        return sale.payments[0].type == "Tarjeta crédito"
      }).length
    },
    cashTotal: function () {
      return this.sales.filter(sale => {
        return sale.payments[0].type == "Efectivo"
      }).length
    },
    monthCreditTotal: function () {
      return this.sales.filter(sale => {
        return sale.date_closed.includes("2019-03") && sale.payments[0].type == "Tarjeta crédito"
      }).length
    },
    monthCashTotal: function () {
      return this.sales.filter(sale => {
        return sale.date_closed.includes("2019-03") && sale.payments[0].type == "Efectivo"
      }).length
    },
    weekCreditTotal: function () {
      return this.sales.filter(sale => {
        return sale.date_closed.includes("2019-03-2") && sale.payments[0].type == "Tarjeta crédito"
      }).length
    },
    weekCashTotal: function () {
      return this.sales.filter(sale => {
        return sale.date_closed.includes("2019-03-2") && sale.payments[0].type == "Efectivo"
      }).length
    },
    allTimeCash: function () {
    var formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    });
    return formatter.format(this.sales.reduce((accumulator, object) => {
         return accumulator + object.total;
    }, 0))
    },
    allMonthCash: function () {
        var formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    });
    return formatter.format(this.sales.filter(sale => {
        return sale.date_closed.includes("2019-03")
    }).reduce((accumulator, object) => {
         return accumulator + object.total;
    }, 0))
    },
    allWeekCash: function () {
        var formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    });
    return formatter.format(this.sales.filter(sale => {
        return sale.date_closed.includes("2019-03-2")
    }).reduce((accumulator, object) => {
         return accumulator + object.total;
    }, 0))
    },
    topProducts: function () {
        var products = this.sales.reduce((accumulator, object) => {
            object.products.forEach(item => {
                if (accumulator[item.category]) {
                    accumulator[item.category].quantity += item.quantity
                } else {
                    accumulator[item.category] = {
                        quantity: item.quantity,
                        category: item.category.toString()
                    }
                }
            })
            return accumulator
        }, {})
        return Object.values(products).sort((a, b) => {
            return b.quantity - a.quantity
        })
    }
}
}
</script>

<style scoped>


.flex-container {
  display: flex;
  flex-flow: row;
  justify-content: space-around;
  padding: 0;
  margin: 0;
  list-style: none;
  
}

.flex-item {
  background: gray;
  width: 100%;
  margin: 1%;
  margin-top: 10px;
  height: 200px;
  color: white;
  flex-grow: 1;
}

h3{
    margin: 20px;
    font-weight: bold;
}

h1{
    text-align: center;
}

.big{
  background: gray;
  width: 100%;
  margin: 1%;
  margin-top: 10px;
  height: 200px;
  color: white;
  flex-grow: 4;
  text-align: center;
}

#top-products{
  list-style: none;
  text-align: left;
  padding: 0;
  padding-bottom: 5%;
  margin: 0% 0% 5% 5%;
  list-style: none;
}
</style>