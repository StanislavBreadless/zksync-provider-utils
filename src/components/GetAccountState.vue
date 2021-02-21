<template>
  <div class="hello">
    <h2>Get account state:</h2>
    
    <input v-model="address" />
    <button @click="getState">Get State!</button>
    <pre v-if="accountState">{{accountState}}</pre>
    <p v-if="errorMessage">{{errorMessage}}</p>
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
      address: '',
      accountState: '',
      errorMessage: ''
    }
  },
  async mounted() {
    const provider = await getDefaultProvider('mainnet');
    this.provider = provider;
  },
  methods: {
    async getState() {
      const provider = this.provider;

      try {
        const state = await provider.getState(this.address);
        this.accountState = JSON.stringify(state, null, 2);
        this.errorMessage = '';
      } catch(e) {
        this.errorMessage = e.message || e;
      }

     // var str = JSON.stringify(obj, null, 2); 
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
