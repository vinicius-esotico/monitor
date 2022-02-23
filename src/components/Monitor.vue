<template>
  <div class="monitor">
    <div class="titulo">
    <h1>{{ title }}</h1>
    <b-button v-b-toggle="apiKey" variant="primary">Mostrar Monitor</b-button>
    <div class="cont">
      <div class="cont-on"> On: {{ cont('on') }} </div>
      <div class="cont-off"> Off: {{ cont('off') }} </div>
    </div>
    <b-collapse v-bind:id="apiKey" class="mt-2">
      <MonitorLoopDown
        :sites="sites"
      />
      <MonitorLoopUp
        :sites="sites"
      />
    </b-collapse>
    <p>{{ error }}</p>
</div>
</div>
</template>

<script>

import MonitorLoopUp from './MonitorLoopUp'
import MonitorLoopDown from './MonitorLoopDown'

import axios from 'axios'

export default {
  props: {
    apiKey: {type: String},
    title: {type: String},
  },
  components: {
    MonitorLoopUp,
    MonitorLoopDown
  },
  data: function () {
        return {
          isUp: null,
          sites: [],
          error: '',
          off: 'off',
          on: 'on',
            headers: { 
              'cache-control': 'no-cache',
              'content-type': 'application/x-www-form-urlencoded' 
            }
        }
},
methods:{
          getMonitors:async function() { 
            try {
            let resp = await axios.post(`https://api.uptimerobot.com/v2/getMonitors?api_key=${this.apiKey}`, this.headers)
            this.sites = resp.data.monitors
            } catch(error) {
              this.error = error
            }
          },
          cont: function(paran) {

            let cont = 0
            
            if(paran == 'on') {
              this.sites.forEach(site => {
                if (site.status === 2) {
                    cont++  
                }
              })
            }else if(paran == 'off') {
              this.sites.forEach(site => {
                if (site.status === 9) {
                    cont++  
                }
              })
            }

            return cont
          },
},
mounted() {
       this.getMonitors()  
  },
  updated() {
    // setInterval(() => {
    //    this.getMonitors()  
    // }, 30000) // 5 minutos em milisegundos = 30000
  }
  
}
</script>

<style>
h1 {
  font-size: 1em;
  padding: 10px;
}

.monitor {
  width: 100%;
  height: fit-content;
}



.cont-on, .cont-off {
  font-size: 24px;
  color: #065c06;
  margin-top: 10px;
  padding: 20px;
  font-weight: bold;
}

.cont-off {
  color: red;
}

.cont {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>