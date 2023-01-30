<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { isPlatformSupported, isBrowserSupported, isAutofillSupported} from '@passwordlessdev/passwordless-client';


const browser = ref("Checking browser support...");
const platform = ref("Checking platform support...")
const autofill = ref("Checking autofill support...")
const browserInfo = ref(window.navigator.userAgent);
onMounted( async () => {
  await checkSupport();
});

const tostring = (bool: boolean) => {
  if(bool) {
    return "supported";
  }
  return "not-supported";
};

const wait = () => {
  return new Promise((resolve) => setTimeout(resolve, 1000));
}

const checkSupport = async () => {

  platform.value = "";

  browser.value ="loading";
  await wait();
  const isBrowser = isBrowserSupported();
  browser.value = tostring(isBrowser);

  platform.value ="loading";
  await wait();

  const isPlatform = await isPlatformSupported();
  platform.value = tostring(isPlatform);

  autofill.value ="loading";
  await wait();
  const isAutofill = await isAutofillSupported();
  autofill.value = tostring(isAutofill);
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

    <div v-if="autofill == 'loading'">Loading...</div>
    <div v-if="autofill == 'supported'">
      ✅ Your browser supports autofill authentication
    </div>
    <div v-if="autofill == 'not-supported'">
    ⚠️ Your browser does not support autofill authentication
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
