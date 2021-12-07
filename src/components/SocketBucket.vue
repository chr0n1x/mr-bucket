<template>
  <br>
  <span><i>
    Just need a test API endpoint? Get one <a href="https://www.piesocket.com/websocket-tester">from Pie Socket</a>
  </i></span>
  <br>

  <div v-if="errors">
    <div class="well error-msg">
      {{ errors }}
    </div>
  </div>

  <div v-if="debug">
    <div class="well">
      {{ debug }}
    </div>
  </div>

  <div class="input-pane">
    <label for="name">Socket Endpoint: </label>
    <input v-model="endpoint" placeholder="<websocket URL>">

    <br/>
    <label for="name">Auth Token: </label>
    <input type="password" v-model="authToken"
      placeholder="<If not set, omits 'Authorization' cookie. Note: this does not dictate how server auths!>">
  </div>

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
      connectionOk: false,
      authToken: "",
      errors: "",
      debug: "",
    }
  },
  methods: {
    disconnect: function() {
      if (!this.connection) { return; }

      this.connectionOk = false;
      this.connection.close();
      this.connection = null;
      this.receivedMessages = [];

      if (!this.debug) {
        this.debug = "Connection to " + this.endpoint + " closed."
      }
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
      this.errors = "";
      this.debug = ""

      console.log("Attempting to connect to " + this.endpoint)
      if (this.authToken) {
        document.cookie = 'Authorization=token ' + this.authToken + '; path=/';
        console.log("Token detected, setting Authorization cookie")
      }
      this.connection = new WebSocket(this.endpoint)

      this.connection.onerror = function(event) {
        this.errors = "Failed to connect: " + JSON.stringify(event)
        this.disconnect();
      }.bind(this)

      this.connection.onclose = function() {
        const lastMessage = this.receivedMessages[this.receivedMessages.length - 1]
        this.debug = "Connection to " + this.endpoint +
          " closed. Last message: " + lastMessage.data
        this.disconnect();
      }.bind(this)

      this.connection.onmessage = function(event) {
        const event_data = JSON.parse(event.data || {})
        if (event_data.type === 'ping') {
          return
        }

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
  .input-pane {
    float: left;
    overflow: hidden;
    width: 100%;
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
      max-height: 340px;
      overflow-y: scroll;
  }
  .error-msg {
    background-color: #FACCCC;
  }
</style>
