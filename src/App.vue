<template>
  <div>
    <div v-for="data in tradeDataList" :key="data.id">
      <div>
        {{ data }}
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data: () => {
      return {
        wss: null,
        tradeDataList: [],

      }
    },

    created() {
      this.getTradeStream();
    },

    methods: {
      getTradeStream() {
        console.log("Starting wss to binance Server");

        this.wss = new WebSocket("wss://stream.binance.com:9443/ws/btcusdt@trade");

        this.wss.addEventListener("message", (event) => {

          let tradeDataString = event.data;
            this.tradeDataList = [];

            let parsedData = JSON.parse(tradeDataString);
            this.tradeDataList = parsedData;

          console.log(this.tradeDataList);
        });

        this.wss.onopen = function (event) {
          //let json = "{\"type\": \"subscribe\",    \"product_ids\": [\"BTC-USD\"]}";
          console.log(event);
          //this.wss.send(json);
          console.log("ICEx market datasını çekiyor");
          //this.wss.send(JSON.stringify({"action":"auth","params":APIKEY}))
          //this.wss.send(JSON.stringify({"action":"subscribe","params":"C.AUD/USD,C.USD/EUR,C.USD/JPY"}))
        };

      }
    }
  }
</script>
