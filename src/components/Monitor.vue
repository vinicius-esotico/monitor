<template>
  <div class="monitor">
    <h1>{{ title }}</h1>
      <div v-for="monitor in sites" :key="monitor.id" class="monitor-row"> 
        <div class="name">
          <span>{{ monitor.friendly_name }}</span>
        </div>
        <div class="url">
          <a>{{ monitor.url }}</a>
        </div>
        <div class="status">
          <span>{{ status(monitor.status) }}</span>
        </div>
      </div>
    <p>{{ error }}</p>
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
      status: null,
      sites: [],
      error: '',
      resp: '',
        headers: { 
          'cache-control': 'no-cache',
          'content-type': 'application/x-www-form-urlencoded' 
        }
    }
},mounted() {
    axios.post(`https://api.uptimerobot.com/v2/getMonitors?api_key=${this.apiKey}`, this.headers)
        .then(resp => {
          [...this.sites] = resp.data.monitors
        }).catch(error => {
          this.error = error
        })

        this.status = i => {
          switch(i) {
            case 0:
              return 'paused'
            case 1:
              return 'not checked yet'
            case 2:
              return 'up'
            case 8:
              return 'seems down'
            case 9:
              return 'down'
          }
        }
      // 5 minutos em milisegundos = 30000
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
}

.monitor-row {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  padding: 10px;
}

</style>