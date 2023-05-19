<template>
    <div class="home">
      <h1 class="title">TFX公链一键发币</h1>
      <MetaMaskConnect />
      <ContractDeployment />
    </div>
</template>
  
<script setup lang="ts">
import MetaMaskConnect from '@/components/MetaMaskConnect.vue';
import ContractDeployment from '@/components/ContractDeployment.vue';

</script>

<style scoped>
.home{
  width: 1000px;
}
.title{
  margin: 20px 0;
}
</style>
  