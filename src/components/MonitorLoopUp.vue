<template>
<div class="monitor-row">
  <div class="loop" v-for="monitor in up" 
        :key="monitor.id" > 
        <div class="on">
        <span class="nome">{{ monitor.friendly_name }}</span>
          <a  
            target="_blank" 
            rel="nofollow" 
            class="link"
            v-bind:href="monitor.url">
            {{ monitor.url }}
          </a>
          <button 
            class="btn-copia" 
            v-on:click="copia(monitor.url)"> 
            <i class="fa-regular fa-copy"></i> 
          </button>
    </div>
  </div>
 </div>
</template>

<script>

export default {
    props: {
      up: {type: Array},
    },
    data() {
      return {

      }
    },
    methods: {
      copia: function(site) {
        let txt = site
        return navigator.clipboard.writeText(txt);
      },
      
    },
    mounted() {
    },
}
</script>

<style scoped>

.loop, .on, .off {
   padding: 10px;
  color: #f1f1f1;
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-radius: 5px;
  width: 100%;
}

.off {
  background-color: red;
}

.on {
  background-color: #065c06;
}

.link {
  color: #f1f1f1;
}
.link:hover {
  color: currentColor;
  text-decoration: none;
}

.btn-copia {
  padding: 8px;
  margin: 10px;
  cursor: pointer;
  border-radius: 5px;
}

</style>