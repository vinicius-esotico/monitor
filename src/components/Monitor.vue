<template>
  <div class="monitor">
    <div class="titulo">
    <h1>{{ title }}</h1>
    <b-button v-b-toggle="apiKey" variant="primary">Mostrar Monitor</b-button>
    <div class="cont">
    <div class="cont-on"> On: {{ cont('on') }} </div>
    <div class="cont-off"> Off: {{ cont('off') }} </div>
    </div>
    </div>
    <b-collapse v-bind:id="apiKey" class="mt-2">
      <div 
      v-for="monitor in sites" 
      :key="monitor.id" 
      class="monitor-row"> 
      
        <div v-bind:class="monitor.status === 9 ? off : on ">
          <div class="name">
              <span>{{ monitor.friendly_name }}</span>
            </div>
            <div class="url">
              <a  
                target="_blank" 
                rel="nofollow" 
                v-bind:href="monitor.url">
                {{ monitor.url }}
              </a>
              <button 
                class="btn-copia" 
                v-on:click="copia(monitor.url)"> 
                <i class="fa-regular fa-copy"></i> 
              </button>
              
            </div>
            <!-- <div class="status">
              <span>{{ status(monitor.status) }}</span>
            </div> -->
            </div>
        </div>
    </b-collapse>
    <p>{{ error }}</p>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  props: {
    apiKey: {type: String},
    title: {type: String},
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
          getMonitors: function() { axios.post(`https://api.uptimerobot.com/v2/getMonitors?api_key=${this.apiKey}`, this.headers)
          .then(resp => {
            [...this.sites] = resp.data.monitors
          }).catch(error => {
            this.error = error
          })
          },
          status: function(i) {
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
          copia: function(site) {
            let txt = site
            return navigator.clipboard.writeText(txt);
          }
},
mounted() {
    // setInterval(() => {
       this.getMonitors()  
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

.monitor-row {
  width: calc(100% - 20px);
  padding: 10px;
}

.name, .url, .status {
  width: calc(100% / 2);
  font-size: 18px;
  vertical-align: middle;
}

.name {
  display: flex;
  justify-content: center;
  align-items: center;
}

.url a {
  color: #f1f1f1;
}

.off {
  background-color: red;
}

.on {
  background-color: #065c06;
}

.on, .off {
  color: #f1f1f1;
  display: flex;
  border-radius: 5px;
  width: 100%;
}

.btn-copia {
  padding: 8px;
  margin: 10px;
  cursor: pointer;
  border-radius: 5px;
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