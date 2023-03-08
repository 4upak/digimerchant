<template>

  <div class="orders">
    <v-card  v-for="message in messages">

      <v-alert
        type="warning"
        :title="message.uid"
        v-if="message.status=='waitPayment' || message.status=='new'"
      >
        {{message.uid}} Ждем оплату
        <span v-if="message.status=='new'">[новая]</span>
      </v-alert>

      <v-alert
        type="error"
        :title="message.uid"
        v-if="message.status=='errorPayout'"
      >
        {{message.uid}} Ошибка выплаты
      </v-alert>

      <v-alert
        type="success"
        :title="message.uid"
        v-if="message.status=='inProgress'"
      >
        {{message.uid}} Клиент оплатил
      </v-alert>

      <v-alert
        type="success"
        :title="message.uid"
        v-if="message.status=='inProgressPayout'"
      >
        {{message.uid}} На выплате
      </v-alert>


        <v-card-title class="headline">
          <!--<v-icon><img :src="'https://digichanger.pro/'+message.route.from.image.files[3].url" /></v-icon>-->
          <img class="route_logo" :src="'https://www.digichanger.pro/service/fs'+message.route.from.image.files[3].url" width="30" />
          {{ message.route.from.name}} ->
          <img class="route_logo"  :src="'https://www.digichanger.pro/service/fs'+message.route.to.image.files[3].url" width="30" />
          {{ message.route.to.name}}

        </v-card-title>
      <v-card-text>
        <div class="status"></div>
        <div class="money">
          {{message.inAmount }} {{message.route.from.symbol}} -> {{message.outAmount }} {{message.route.to.symbol}}
          <div class="client">
            <div class="contacts" v-for="item in message.routeValues" v-if="item">
              <span v-if="item.value !='' ">{{item.name}}: {{item.value}}</span>
            </div>
          </div>
        </div>

      </v-card-text>
    </v-card>

    <div>
      <audio ref="audioPlayer">
        <source src="https://www.zapsplat.com/wp-content/uploads/2015/sound-effects-77317/zapsplat_multimedia_button_click_fast_wooden_organic_005_78839.mp3" type="audio/mp3">
      </audio>
    </div>
  </div>
  <!---->
</template>


<script>
import "@/assets/style.css";
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

      // sotr by status
      data.message.sort(function(a, b) {
        if (a.status < b.status) {
          return -1;
        }
        if (a.status > b.status) {
          return 1;
        }
        return 0;
      });
      if (data.message.length != this.messages.length) {
        console.log('new message');
        this.$refs.audioPlayer.play();
      }
      //var audio = document.getElementById("myAudio");
      //audio.play();

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
