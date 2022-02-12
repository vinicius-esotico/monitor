<template>
  <div class="monitor">
    <h1>{{ title }}</h1>
    <p>API: {{ apiKey }}</p>
    <p>Data: {{ data }}</p>
    <p>Error: {{ error }}</p>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  props: {
    apiKey: {type: String},
    title: {type: String},
  },data: function () {
    return {
      data: null,
      error: '',
      config: {
        method: 'POST',
        url: 'https://api.uptimerobot.com/v2/getMonitors/',
        headers: { 
          'cache-control': 'no-cache',
          'content-type': 'application/x-www-form-urlencoded' 
          },
        form: { 
          api_key: this.apiKey, format: 'json', logs: '1' 
        }
      }
    }
},
  mounted() {
    axios.get(this.config)
          .then(response => {
            this.data = response
          }).catch(error => {
            this.error = error
          })
  }
}
</script>

<style>
h1 {
  font-size: 2em;
  padding: 20px;
}

.monitor {
  width: 100%;
  height: fit-content;
  background-color: #ccc;

}

</style>