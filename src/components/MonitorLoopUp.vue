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

<style>

</style>