<template>
  <div class="monitor">
    <div class="titulo">
    <h1>{{ title }}</h1>
    </div>
    <b-button class="btn" v-b-toggle="apiKey" variant="primary">Mostrar Monitor</b-button>
    <div class="cont">
      <div v-if="cont('off') != 0" class="cont-off"> Off: {{ cont('off') }} </div>
      <div class="cont-on"> On: {{ cont('on') }} </div>
    </div>
    <b-collapse v-bind:id="apiKey" class="mt-2">
      <MonitorLoopDown
        :down="sites.filter(this.filtroDown)"
      />
      <MonitorLoopUp
        :up="sites.filter(this.filtroUp)"
      />
    </b-collapse>
    <p>{{ error }}</p>
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
          filtroDown: function(obj) {
            return obj.status == 9
          },
          filtroUp: function(obj) {
            return obj.status !== 9
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

<style scoped>
h1 {
  font-size: 1.5em;
  padding: 10px;
}

.monitor {
  display: flex;
  box-sizing: border-box;
  width: 100%;
  justify-content: space-around;
  vertical-align: middle;
  margin: 10px 0;
  flex-wrap: wrap;
}

.btn {
  widows: fit-content;
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