<template>
  <div class="hello">
    <h2>zkSync Prices (not realtime):</h2>
    <table v-if="txInfo.length !== 0">
      <thead>
      <tr>
        <th>Tx type</th>
        <th class="ml-1">Price (ETH)</th>
        <th class="ml-1">Price (USD)</th>
      </tr>
      </thead>
      <tbody>
        <tr v-for="(tx, index) in txInfo" v-bind:key="index">
          <td>{{tx.name}}</td>
          <td class="ml-1">{{tx.priceETH}}</td>
          <td class="ml-1">{{tx.priceUSD}}</td>
        </tr>
      </tbody>
    </table>
    <p v-else>loading...</p>
  </div>
</template>

<script>

import { getDefaultProvider } from 'zksync';

const ADDRESS = '0xc0f97CC918C9d6fA4E9fc6be61a6a06589D199b2';
// I just hope vanity-eth.tk is random enough
const NEW_ADDRESS = '0x12127131898a709D5cD2F4Ec0f6C732D5EBa3475';

export default {
  name: 'HelloWorld',
  data() {
    return {
      txInfo: [],
      provider: null 
    }
  },
  async mounted() {
    const provider = await getDefaultProvider('mainnet');
    this.provider = provider;

    const txs = [];

    txs.push({
        name: 'Transfer',
        priceETH: await this.getFormattedPrice("Transfer",ADDRESS,"ETH"),
    });
    txs.push({
        name: 'TransferToNew',
        priceETH: await this.getFormattedPrice("Transfer",NEW_ADDRESS,"ETH"),
    });
    txs.push({
        name: 'Withdraw',
        priceETH: await this.getFormattedPrice("Withdraw",ADDRESS,"ETH"),
    });
    txs.push({
        name: 'Withdraw (fast)',
        priceETH: await this.getFormattedPrice("FastWithdraw",ADDRESS,"ETH"),
    });
    txs.push({
        name: 'ChangePubKey (ECDSA)',
        priceETH: await this.getFormattedPrice({"ChangePubKey": "ECDSA" },ADDRESS,"ETH"),
    });
    txs.push({
        name: 'ChangePubKey (onchainAuth)',
        priceETH: await this.getFormattedPrice({"ChangePubKey": "Onchain" },ADDRESS,"ETH"),
    });
    txs.push({
        name: 'ChangePubKey (CREATE2)',
        priceETH: await this.getFormattedPrice({"ChangePubKey": "CREATE2"},ADDRESS,"ETH"),
    })

    const ethPrice = await provider.getTokenPrice('ETH');

    this.txInfo = txs.map(tx => ({
      ...tx,
      priceUSD: (+tx.priceETH * ethPrice).toFixed(2)
    }));
  },
  methods: {
    async getFormattedPrice(
      tx, address, token
    ) {
      const provider = this.provider;

      const price = (await provider.getTransactionFee(
        tx,
        address,
        token
      )).totalFee;

      return provider.tokenSet.formatToken(token, price);
    },
    async initAllPrices() {

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}

.ml-1 {
  padding-left: 20px;
}
</style>
