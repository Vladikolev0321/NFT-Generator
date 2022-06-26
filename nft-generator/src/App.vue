<template>
  <main>
    <router-view></router-view>
  </main>
</template>

<script>
const Web3 = require('web3');

export default {
  name: 'App',
  components: {
  },
  created() {
    this.ethEnabled();
  },
  methods: {
    async ethEnabled() {
      if (window.ethereum) {
        await window.ethereum.request({ method: 'eth_requestAccounts' });
        this.web3 = new Web3(window.ethereum);
        this.accounts = await this.web3.eth.getAccounts();
        // get user account balance
        this.web3.eth.getBalance(this.accounts[0]).then((balance) => {
          console.log(balance);
        });
        return true;
      }
      return false;
    },
  },

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
