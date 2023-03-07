<template>
  <v-row justify="center" align="center">
    hello!
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      ws: null
    }
  },
  mounted() {
    this.ws = new WebSocket('ws://167.235.77.8:8010/ws/emulator/controler/');

    this.ws.addEventListener('open', () => {
      //console.log('WebSocket connection opened');
    });

    this.ws.addEventListener('message', (event) => {
      console.log('WebSocket message received:', event.data);


    });

    this.ws.addEventListener('error', (event) => {
      //console.error('WebSocket error:', event);
    });

    this.ws.addEventListener('close', (event) => {
      //console.log('WebSocket connection closed:', event);
    });
  },
}
</script>
