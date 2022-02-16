<template>
  <div class="main">
    <Monitor 
    apiKey="ur1615701-d183bc4f26c1d1195e6b0340"
    title="Monitor Esotico.com.br"
    />
    <Monitor 
    apiKey="ur589369-a2a47145b05620158e902ad7"
    title="Monitor HostHP"
    />
  </div>
</template>

<script>

import Monitor from '../components/Monitor'

export default {
  components: {
    Monitor
  }
}
</script>

<style>
.main {
  display: flex;
  justify-content: center;
  flex-direction: column;
}
</style>