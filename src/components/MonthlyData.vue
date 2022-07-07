<template>
    <div class="monthly-data-panel">
        <h2>Tu 2019 Hasta ahora!</h2>
        <h5> Enero </h5>
        <b-progress :value="januarySales" :max="max" show-value variant="secondary" class="mb-3" id="first"></b-progress>
        <h5> Febrero </h5>
        <b-progress :value="februarySales" :max="max" show-value variant="secondary" class="mb-3" id="second"></b-progress>
        <h5> Marzo </h5>
        <b-progress :value="marchSales" :max="max" show-value variant="secondary" class="mb-3" id="third"></b-progress>
        <b-tooltip target="first"> Numero de ventas: {{january}} </b-tooltip>
        <b-tooltip target="second"> Numero de ventas: {{february}} </b-tooltip>
        <b-tooltip target="third"> Numero de ventas: {{march}} </b-tooltip>
    </div>
</template>

<script>
export default {
    name: 'MonthlyData',
    props:
    {
        sales: {
            type: Array
        }
    },
    setup() {
        
    },
    data() {
        return {
            value: 0,
        }
    },
    computed: {
        january: function () {
            return this.sales.filter(sale => {
                return sale.date_closed.includes("2019-01")
            }).length
        },

        januarySales: function () {
            return this.sales.filter(sale => {
                return sale.date_closed.includes("2019-01")
            }).reduce((acc, sale) => {
                return acc + sale.total
            }, 0)
        },
        february: function () {
            return this.sales.filter(sale => {
                return sale.date_closed.includes("2019-02")
            }).length
        },
        februarySales: function () {
            return this.sales.filter(sale => {
                return sale.date_closed.includes("2019-02")
            }).reduce((acc, sale) => {
                return acc + sale.total
            }, 0)
        },
        march: function () {
            return this.sales.filter(sale => {
                return sale.date_closed.includes("2019-03")
            }).length
        },
        marchSales: function () {
            return this.sales.filter(sale => {
                return sale.date_closed.includes("2019-03")
            }).reduce((acc, sale) => {
                return acc + sale.total
            }, 0)
        },
        max: function () {
            return Math.max(this.januarySales, this.februarySales, this.marchSales)
        }
     }
}
</script>

<style scoped>
h5{
    text-align: left;
}

</style>