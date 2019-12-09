<!-- myapp/src/components/Calculator.vue-->
<!-- eslint-disable-next-line-->
<!-- eslint-disable -->
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
          <div class>Calculator</div>
          <table border="1">
            <tr>
              <td colspan="3">
                <input type="text" v-model="message" />
              </td>
              <!-- clr() function will call clr to clear all value -->
              <td>
                <input type="button" value="c" v-on:click="clr()" />
              </td>
            </tr>
            <tr>
              <!-- create button and assign value to each button -->
              <!-- dis("1") will call function dis to display value -->
              <td>
                <input type="button" value="1" v-on:click="dis('1')" />
              </td>
              <td>
                <input type="button" value="2" v-on:click="dis('2')" />
              </td>
              <td>
                <input type="button" value="3" v-on:click="dis('3')" />
              </td>
              <td>
                <input type="button" value="/" v-on:click="dis('/')" />
              </td>
            </tr>
            <tr>
              <td>
                <input type="button" value="4" v-on:click="dis('4')" />
              </td>
              <td>
                <input type="button" value="5" v-on:click="dis('5')" />
              </td>
              <td>
                <input type="button" value="6" v-on:click="dis('6')" />
              </td>
              <td>
                <input type="button" value="-" v-on:click="dis('-')" />
              </td>
            </tr>
            <tr>
              <td>
                <input type="button" value="7" v-on:click="dis('7')" />
              </td>
              <td>
                <input type="button" value="8" v-on:click="dis('8')" />
              </td>
              <td>
                <input type="button" value="9" v-on:click="dis('9')" />
              </td>
              <td>
                <input type="button" value="+" v-on:click="dis('+')" />
              </td>
            </tr>
            <tr>
              <td>
                <input type="button" value="." v-on:click="dis('.')" />
              </td>
              <td>
                <input type="button" value="0" v-on:click="dis('0')" />
              </td>
              <!-- solve function call function solve to evaluate value -->
              <td>
                <input type="button" value="=" v-on:click="solve()" />
              </td>
              <td>
                <input type="button" value="*" v-on:click="dis('*')" />
              </td>
            </tr>
          </table>
          <br />
        </md-field>
        <md-field>
          <label>Calculations</label>
          <md-textarea v-model="textarea" disabled></md-textarea>
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
      count: 0
    };
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
      this.textarea += data + "\n"; // append each new message to the textarea and add a line break
    }
  },
  methods: {
    dis(val) {
      this.message += val;
    },
    solve() {
      let x = this.message;
      let y = eval(x);
      this.message += " = " + y;
      this.$socket.emit("message", this.message);
      this.message = "";
    },
    clr() {
      this.message = ""; // empty the message bar
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
