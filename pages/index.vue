<template>
  <v-row justify="center" align="center">
    <v-list-item v-for="message in messages">
      <v-list-item-content>
        <v-list-item-title >{{ message.route.from.name}}: {{message.inAmount }}</v-list-item-title>
      </v-list-item-content>
      <v-list-item-content>
        <v-list-item-title >{{ message.route.to.name}}: {{message.outAmount }}</v-list-item-title>
      </v-list-item-content>
      <v-list-item-content>
        <v-list-item-title >{{ message.status }}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      ws: null,
      messages: [],
    }
  },
  mounted() {
    this.ws = new WebSocket('ws://167.235.77.8:8010/ws/emulator/controler/');

    this.ws.addEventListener('open', () => {
      //console.log('WebSocket connection opened');
    });

    this.ws.addEventListener('message', (event) => {
      //console.log('WebSocket message received:', event.data);
      //parse json
      var data = JSON.parse(event.data);
      //console.log(data.message);
      this.messages=data.message;
      console.log(this.messages);



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
