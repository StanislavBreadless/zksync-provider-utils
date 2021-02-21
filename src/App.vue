<template>
  <div id="app">
    <prices />
    <get-account-state />
    <get-eth-tx-for-withdraw />
  </div>
</template>

<script>
import Prices from './components/Prices.vue'
import GetAccountState from './components/GetAccountState';
import GetEthTxForWithdraw from './components/GetEthTxForWithdraw';

export default {
  name: 'App',
  components: {
    Prices,
    GetAccountState,
    GetEthTxForWithdraw
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
