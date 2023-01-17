<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { isPlatformSupported,isBrowserSupported } from '@passwordlessdev/passwordless-client';


const browser = ref("Checking browser support...");
const platform = ref("Checking platform supported...")
const browserInfo = ref(window.navigator.userAgent);
onMounted( async () => {
  await checkSupport();
});

const checkSupport = async () => {

  platform.value = "";

  browser.value ="loading";
  await new Promise((resolve) => setTimeout(resolve, 1000));
  const isBrowser = await isBrowserSupported();
  if(isBrowser) {
    browser.value = "supported";
  } else {
    browser.value = "not-supported";
  }

  platform.value ="loading";
  await new Promise((resolve) => setTimeout(resolve, 1000));

  const isPlatform = await isPlatformSupported();
  if(isPlatform) {
    platform.value = "supported";
  } else {
    platform.value = "not-supported";
  }

  browserInfo.value
}


</script>

<template>
  <div>
    <h1>Checking your support</h1>

    <div v-if="browser == 'loading'">Loading...</div>
    <div v-if="browser == 'supported'">
    ✅ Your browser supports WebAuthn<br />
    ✅ Your browser supports Security Keys
    </div>
    <div v-if="browser == 'not-supported'">
    ⚠️ Your browser does not support WebAuthn
    ⚠️ Your browser does not support Security Keys
    </div>

    <div v-if="platform == 'loading'">Loading...</div>
    <div v-if="platform == 'supported'">
      ✅ Your browser supports Platform authentication<br />
      (e.g. FaceId, TouchId, Windows Hello)
    </div>
    <div v-if="platform == 'not-supported'">
    ⚠️ Your browser does not support Platform authentication<br />
    (e.g. FaceId, TouchId, Windows Hello)
    </div>

    <p>{{ browserInfo }}</p>
    
    <button @click="checkSupport">Check again</button>
  </div>
  <HelloWorld msg="Vite + Vue" />
</template>

<style scoped>
button {
  margin-top:20px;
}
</style>
