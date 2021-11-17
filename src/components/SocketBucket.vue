<template>
  <br>
  <span><i>
    Just need a test API endpoint? Get one <a href="https://www.piesocket.com/websocket-tester">from Pie Socket</a>
  </i></span>
  <br>

  <label for="name">Socket Endpoint: </label>
  <input v-model="endpoint" placeholder="<websocket URL>">

  <span v-if="!connectionOk"> ❌ </span>
  <span v-if="connectionOk"> ✅ </span>
  <button v-if="!connection" v-on:click="startConnection">Start Connection</button>
  <button v-if="connection" v-on:click="disconnect">End Connection</button>

  <br>
  <button v-if="connection" v-on:click="clearMessages" class="msg-format-ctl">Clear Message History</button>

  <div v-if="connection" class="well">
    <pre v-for="(item, index) in receivedMessages" v-bind:item="item" v-bind:index="index" v-bind:key="item.timestamp">
      {{ item.data }}
    </pre>
  </div>
</template>

<script>
export default {
  name: 'SocketBucket',
  data: function() {
    return {
      endpoint: null,
      connection: null,
      receivedMessages: [],
      connectionOk: false
    }
  },
  methods: {
    disconnect: function() {
      if (!this.connection) { return; }

      console.log("Closing connection to " + this.endpoint);
      this.connectionOk = false;
      this.connection.close();
      this.connection = null;
      this.endpoint = "";
      this.receivedMessages = [];
    },

    clearMessages: function() {
      this.receivedMessages = [];
    },

    appendMessage: function(event) {
      this.receivedMessages.push(event)
    },

    startConnection: function() {
      if (this.connection) { this.disconnect() }

      this.receivedMessages = [];
      console.log("Starting connection to WebSocket Server")
      this.connection = new WebSocket(this.endpoint)

      this.connection.onmessage = function(event) {
        this.appendMessage(event)
      }.bind(this)

      this.connection.onopen = function(event) {
        console.log("Successfully connected to the echo websocket server...")
        this.connectionOk = true;
        this.appendMessage(event)
      }.bind(this)
    }
  }
}
</script>

<style scoped>
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  input {
    width: 50%;
    padding-right: 2;
  }
  .msg-format-ctl {
    float: right;
    overflow: hidden;
    margin-top: 20px;
  }
  .well {
      min-height: 20px;
      padding: 19px;
      margin-bottom: 20px;
      margin-top: 20px;
      background-color: #f5f5f5;
      border: 1px solid #e3e3e3;
      border-radius: 4px;
      -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.05);
      box-shadow: inset 0 1px 1px rgba(0,0,0,.05);
  }
</style>
