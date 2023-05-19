<template>
  <!-- <button @click="connect">连接 MetaMask</button> -->
</template>

<script setup lang="ts">
import {ethers} from 'ethers';
import { onMounted, ref } from 'vue';

const signer = ref({})
const balance = ref('')

const connect = async() => {
  const { ethereum } = (window as any);
      if (ethereum ) {
  
        let web3Provider: any;
        web3Provider = new ethers.providers.Web3Provider(ethereum);
        const accounts = await web3Provider.send("eth_requestAccounts", []);
        let _account: string = accounts[0].substr(0, 6) + '...' + accounts[0].substr(-4);
        console.log('当前账号是',_account);
        signer.value = await web3Provider.getSigner();

        try {
          let _balance = await web3Provider.getBalance(accounts[0]);
          _balance = _balance / 10 ** 18;
          balance.value = _balance.toString();
          console.log('当前 ETH 余额是',balance.value)
        } catch (e) {
          console.error(e);
        }
      }
    
};

onMounted(()=>connect());
</script>
