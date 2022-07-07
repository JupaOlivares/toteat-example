<template>
  <div id="app">
    <div class="side"></div>
    <div class="main">
      <h1> Dashboard "La Pikada De La Esquina" </h1>
      <hr>
      <h2> Estadisticas </h2>
        <StatisticsPanel :sales="sales"></StatisticsPanel>
      <hr>
      <h2> Últimas Transacciones </h2>
      <div class="components-row-1">
        <OrderTable :sales="sales"> </OrderTable>
              <hr>
        <MonthlyData :sales="sales"> </MonthlyData>
      </div>

    </div>
    <div class="side"></div>
  </div>
</template>

<script>

import OrderTable from './components/OrderTable'
import sales from "./data/ventas.json"
import StatisticsPanel from './components/StatisticsPanel'
import MonthlyData from './components/MonthlyData'

export default {
  name: 'App',
  components: {
    OrderTable,
    StatisticsPanel,
    MonthlyData
  },
  data: function () {
    return {
      sales: sales
    }
  }}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: row;
}
.side{
  flex-grow: 1;
}

.main{
  margin: 10px;
  flex-grow: 8;
  display: flex;
  flex-direction: column;
}
h1{
  text-align: left;
  font-size: 40px;
  margin: 5%;
}
h2{
  text-align: left;
  font-size: 30px;
  margin: 5% 5%;
}
</style>
