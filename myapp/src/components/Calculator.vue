<!-- myapp/src/components/Calculator.vue-->
<template>
  <div class="page-container">
    <md-app>
      <md-app-toolbar class="md-primary">
        <div class="md-toolbar-row">
          <span class="md-title">Calculator</span>
        </div>
      </md-app-toolbar>
      <md-app-content>
        <md-field>
          <table border="1">
            <tr>
              <td colspan="3">
                <input type="text" v-model="message" />
              </td>
              <!-- clear() function will call clear to clear all value -->
              <td>
                <input type="button" value="c" v-on:click="clear()" />
              </td>
            </tr>
            <tr>
              <!-- create button and assign value to each button -->
              <!-- display("1") will call function display to display value -->
              <td>
                <input type="button" value="1" v-on:click="display('1')" />
              </td>
              <td>
                <input type="button" value="2" v-on:click="display('2')" />
              </td>
              <td>
                <input type="button" value="3" v-on:click="display('3')" />
              </td>
              <td>
                <input type="button" value="/" v-on:click="display(' / ')" />
              </td>
            </tr>
            <tr>
              <td>
                <input type="button" value="4" v-on:click="display('4')" />
              </td>
              <td>
                <input type="button" value="5" v-on:click="display('5')" />
              </td>
              <td>
                <input type="button" value="6" v-on:click="display('6')" />
              </td>
              <td>
                <input type="button" value="-" v-on:click="display(' - ')" />
              </td>
            </tr>
            <tr>
              <td>
                <input type="button" value="7" v-on:click="display('7')" />
              </td>
              <td>
                <input type="button" value="8" v-on:click="display('8')" />
              </td>
              <td>
                <input type="button" value="9" v-on:click="display('9')" />
              </td>
              <td>
                <input type="button" value="+" v-on:click="display(' + ')" />
              </td>
            </tr>
            <tr>
              <td>
                <input type="button" value="." v-on:click="display('.')" />
              </td>
              <td>
                <input type="button" value="0" v-on:click="display('0')" />
              </td>
              <!-- solve function call function solve to evaluate value -->
              <td>
                <input type="button" value="=" v-on:click="solve()" />
              </td>
              <td>
                <input type="button" value="*" v-on:click="display(' * ')" />
              </td>
            </tr>
          </table>
          <br />
        </md-field>
        <md-field>
          <label>Calculations</label>
          <md-textarea v-model="textarea" disabled></md-textarea>
          <md-button v-on:click="clearHistory()">Clear History</md-button>
        </md-field>
      </md-app-content>
    </md-app>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      textarea: "",
      message: "",
      count: 0,
      messages: [],
      maxMessages: 10
    };
  },
  mounted() {
    if (localStorage.getItem("messages")) {
      try {
        let index = 0;
        this.messages = JSON.parse(localStorage.getItem("messages"));
        for (index = 0; index < this.messages.length; index++) {
          this.textarea += this.messages[index] + "\n";
        }
      } catch (e) {
        localStorage.removeItem("messages");
      }
    }
  },
  sockets: {
    connect() {
      console.log("connected to server");
    },
    count(val) {
      this.count = val.count;
    },
    message(data) {
      // this function gets triggered once a socket event of `message` is received
      this.textarea = data + "\n" + this.textarea; // append each new message to the textarea and add a line break
      if (this.messages.length >= this.maxMessages) {
        this.messages.pop();
      }
      this.messages.unshift(data);
      this.saveMessages();
    }
  },
  methods: {
    display(val) {
      this.message += val;
    },
    solve() {
      let x = this.message;
      let y = eval(x);
      this.message += " = " + y;
      this.$socket.emit("message", this.message);
      this.message = "";
    },
    clear() {
      this.message = ""; // empty the message bar
    },
    clearHistory() {
      this.messages = [];
      this.textarea = "";
      localStorage.removeItem("messages");
    },
    saveMessages() {
      const parsed = JSON.stringify(this.messages);
      localStorage.setItem("messages", parsed);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.md-app {
  height: 800px;
  border: 1px solid rgba(#000, 0.12);
}
.md-textarea {
  height: 300px;
}
</style>
