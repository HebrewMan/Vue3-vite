<template>
  <div class="args">
    <div>
      <p>* 代币名称</p>
      <input v-model="name" type="text" placeholder="长度在2-11个英文字符"/>
    </div>

    <div>
      <p>* 代币符号</p>
      <input v-model="symbol" placeholder="3~4大写英文字符,例如ETHBTCBAT等不可包含空格" type="text" />
    </div>

    <div>
      <p>* 小数位数</p>
      <input v-model="decimals" placeholder="小数位数默认18"  type="text" />
    </div>

    <div>
      <p>* 总发行量</p>
      <input v-model="totalSupply" placeholder="要发行的代币总量，最小1，最大1000000000000000" type="text" />
    </div>

    <div>
      <button @click="deployContract" >发行代币</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { ethers } from 'ethers';
import {abi,bytecode} from '@/artifacts/ERC20Token.ts';

const name = ref('');
const symbol = ref('');
const totalSupply = ref('');
const decimals = ref('18');

const deployContract = async() => {

  if (typeof (window as any).ethereum !== 'undefined') {

    const ethereum = (window as any).ethereum;
    const provider = new ethers.providers.Web3Provider(ethereum);
    const signer = provider.getSigner(); 

    try {
      const contractFactory = new ethers.ContractFactory(abi,bytecode,signer);
      contractFactory.deploy(
        name.value,
        symbol.value,
        totalSupply.value,
        decimals.value,
      ).then(async(contract)=>{

        console.log('合约地址',contract.address);
        console.log('代币名称',await contract.name());
        console.log('代币符号',await contract.symbol());
        console.log('小数位数',await contract.decimals());
        console.log('总发行量',(await contract.totalSupply())*1);

      })
    } catch (error) {
      console.error('部署失败:', error);
    }

  }else{
    console.error('MetaMask 钱包未安装')
  }
};
</script>

<style scoped>
.args{
  background: #fff;
  border-radius: 5px;
  padding: 20px 0;
}
.args div{
  text-align: left;
  box-sizing: border-box;
  padding:0 20px;
  font-size: 13px;
}
input{
  width: 100%;
  height: 30px;
  box-sizing: border-box;
  font-size: 13px;
  border:none;
  border: 1px solid #D3d3d3;
  border-radius: 3px;
  padding-left: 5px;
}

input::placeholder {
    opacity: 0.4;
}

input:focus{
  outline: none;
  border: 1px solid skyblue;
}

button{
  margin-top: 30px;
  background: yellow;
  font-weight: bold;
  font-size: 14px;
}
</style>
