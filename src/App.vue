<template>
  <div id="app">
    <h1>[Bitbank] Realtime data</h1>
    <p class="text-right">本页面使用Bitbank提供的API数据。</p>
    <p class="text-right">
      <a href="https://twitter.com/tarotaro080808" target="_blank">tarotaro080808</a>
    </p>

    <div class="panel">
      <depth/>
    </div>
    <div class="panel">
      <sell-buy-ratio :latestItem="latestItem"/>
    </div>
    <div class="panel">
      <transactions :items="items"/>
    </div>
  </div>
</template>

<script>
import PubNub from 'pubnub'
import Transactions from '@/components/Transactions'
import Depth from '@/components/Depth'
import SellBuyRatio from '@/components/SellBuyRatio'

export default {
  name: 'app',
  data: function() {
    return {

      items: [],
      latestItem: {}
    }
  },
  components: {
    Transactions,
    Depth,
    SellBuyRatio
  },
  mounted: function() {
    const SUBSCRIBE_KEY = 'sub-c-e12e9174-dd60-11e6-806b-02ee2ddab7fe'
    const TRANSACTIONS_CHANNEL = 'transactions_xrp_jpy'
    // const DEPTH_CHANNEL = 'depth_xrp_jpy'
    const pubnub = new PubNub({
      subscribeKey: SUBSCRIBE_KEY
    })
    pubnub.addListener({
      message: message => {
        // if (message.channel == DEPTH_CHANNEL) {
        // }
        if (message.channel == TRANSACTIONS_CHANNEL) {
          this.latestItem = Object.assign({}, message.message.data.transactions[0])
          this.items.unshift(message.message.data.transactions[0])
          if (this.items.length > 100) {
            this.items.pop()
          }
        }
      }
    })
    pubnub.subscribe({
      channels: [TRANSACTIONS_CHANNEL]
    })
  }
}
</script>
<style>
*,
:after,
::before {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
body {
  background: #1e1e2f;
  color: #ffffff;
}
a {
  color: #ffffff;
  text-decoration: underline;
}
.align-right {
  text-align: right;
}
.panel {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 0.0625rem solid rgba(34, 42, 66, 0.05);
  border-radius: 0.2857rem;
  background: #27293f;
  position: relative;
  width: 100%;
  margin-bottom: 25px;
  box-shadow: 0 1px 20px 0 rgba(0, 0, 0, 0.1);
  padding: 15px;
}
.panel-header {
  padding: 15px 15px 0;
  border: 0;
  color: #fff;
}
.text-right {
  text-align: right;
  font-size: 12px;
  color: #ffffff;
}
.heading {
  font-weight: bold;
  font-size: 1.15rem;
}
#app {
  width: 800px;
  max-width: 100%;
  margin-left: auto;
  margin-right: auto;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 10px;
}
h1 {
  font-size: 20px;
  margin-bottom: 20px;
}
table {
  width: 320px;
  max-width: 100%;
  margin-left: auto;
  margin-right: auto;
}
td {
  padding: 5px;
}
.type-def {
  color: #db524b;
}
.type {
  color: #029688;
}
</style>
