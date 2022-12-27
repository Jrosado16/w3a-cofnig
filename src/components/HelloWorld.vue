<template>
  <div>
    <h2>Web3Auth X Vue.js</h2>
    <section>
      <button class="rpcBtn" @click="login" style="cursor: pointer">Login</button>
      <!-- <button class="rpcBtn" @click="getUserInfo" style="cursor: pointer">Get User Info</button> -->
      <!-- <button class="rpcBtn" @click="getChainId" style="cursor: pointer">Get Chain ID</button> -->
      <!-- <button class="rpcBtn" @click="getAccounts" style="cursor: pointer">Get Accounts</button> -->
      <!-- <button class="rpcBtn" @click="getBalance" style="cursor: pointer">Get Balance</button> -->
      <!-- <button class="rpcBtn" @click="sendTransaction" style="cursor: pointer">Send Transaction</button> -->
      <!-- <button class="rpcBtn" @click="signMessage" style="cursor: pointer">Sign Message</button> -->
      <!-- <button class="rpcBtn" @click="getPrivateKey" style="cursor: pointer">Get Private Key</button> -->
      <button class="rpcBtn" @click="logout" style="cursor: pointer">Logout</button>
    </section>
  </div>
</template>

<script>

import { ref, onMounted } from "vue";
import { Web3Auth } from "@web3auth/modal";
import { CHAIN_NAMESPACES } from "@web3auth/base";

export default {
  name: "Ho-me",
  props: {
    msg: String,
  },
  setup() {
    const loading = ref(false);
    const loginButtonStatus = ref("");
    const connecting = ref(false);
    let provider = ref(null);

    const clientId = "BHg_WXvNtmEGDBwGfJ5nPUf1J9JvKM_UJB_9sOCSK3wHNXfEEV4rsePnuDt0oTr1vSEBk22cq-tXoXlbHwmXUys"; // get from https://dashboard.web3auth.io

      const web3auth = new Web3Auth({
        clientId,
        chainConfig: {
          chainNamespace: CHAIN_NAMESPACES.EIP155,
          chainId: "0x1",
          rpcTarget: "https://rpc.ankr.com/eth", // This is the public RPC we have added, please pass on your own endpoint while creating an app
        },
      });

    onMounted(async () => {
      try {
        loading.value = true;


      await web3auth.initModal();
        if (web3auth.provider) {
          provider = web3auth.provider;
        }
      } catch (error) {
        console.log("error", error);
        console.log("error", error);
      } finally {
        loading.value = false;
      }
    });

    const login = async () => {
      if (!web3auth) {
        console.log("web3auth not initialized yet");
        return;
      }
      provider = await web3auth.connect();
      console.log('provider: ', provider);
    };

    return {
      loading,
      loginButtonStatus,
      connecting,
      provider,
      web3auth,
      login,
    };
  },
};
</script>
