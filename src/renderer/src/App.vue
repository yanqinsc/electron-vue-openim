<script setup lang="ts">
// import Versions from "./components/Versions.vue";

import { getSDK, CbEvents } from "open-im-sdk-wasm";

const isElectron = window.electron !== undefined;
const IMSDK = getSDK({
    coreWasmPath: "./openIM.wasm",
    sqlWasmPath: (isElectron ? ".." : "") + "/sql-wasm.wasm" ,
    debug: true
  });
IMSDK.on(CbEvents.OnConnecting, () => {
  // 连接中
});
IMSDK.on(CbEvents.OnConnectSuccess, () => {
  // 连接成功
  console.log('连接成功');
});
IMSDK.on(CbEvents.OnConnectFailed, () => {
  console.log('连接失败');
  // 连接失败
});
IMSDK.on(CbEvents.OnUserTokenExpired, () => {
  // token无效
});


IMSDK.login({
  userID: "111557",
  token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVc2VySUQiOiIxMTE1NTciLCJQbGF0Zm9ybUlEIjo1LCJleHAiOjE3Mjg4MjYzOTksIm5iZiI6MTcyMTA1MDA5OSwiaWF0IjoxNzIxMDUwMzk5fQ.7qs8MoyfhZBd1QK0NS4J5DkP7co26THnKhauVIHBLio",
  platformID: 5,
  apiAddr: "http://47.251.25.186:10002",
  wsAddr: "ws://47.251.25.186:10001"
})
  .then(() => {
    console.log('login success');
  })
  .catch((err) => {
    console.log(err);
  });
const ipcHandle = () => window.electron.ipcRenderer.send("ping");
</script>

<template>
  <img alt="logo" class="logo" src="./assets/electron.svg" />
  <div class="creator">Powered by electron-vite</div>
  <div class="text">
    Build an Electron app with
    <span class="vue">Vue</span>
    and
    <span class="ts">TypeScript</span>
  </div>
  <p class="tip">Please try pressing <code>F12</code> to open the devTool</p>
  <div class="actions">
    <div class="action">
      <a href="https://electron-vite.org/" target="_blank" rel="noreferrer">Documentation</a>
    </div>
    <div class="action">
      <a target="_blank" rel="noreferrer" @click="ipcHandle">Send IPC</a>
    </div>
  </div>
</template>
