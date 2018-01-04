<template>
  <div id="app">
    <md-whiteframe>
      <md-toolbar>
        <span class="md-title">Secure Password Generator</span>
      </md-toolbar>
    </md-whiteframe>

    <div class="page-layout">
      <div class="container">
        <md-input-container>
          <label>Password</label>
          <md-input type="text" v-model="password" ></md-input>
        </md-input-container>
        <md-input-container>
          <label>Password Length</label>
          <md-input type="number" v-model="passwordLength"></md-input>
        </md-input-container>
        <div class="switch-container">
          <md-switch class="md-primary" v-model="includeSymbols">Include Symbols, e.g. @#$%</md-switch>
          <md-switch class="md-primary" v-model="includeNumbers">Include Numbers, e.g. 123456</md-switch>
          <md-switch class="md-primary" v-model="includeLowercaseCharacters">Include Lowercase Characters, e.g. abcdefgh</md-switch>
          <md-switch class="md-primary" v-model="includeUppercaseCharacters">Include Uppercase Characters, e.g. ABCDEFGH</md-switch>
        </div>
        <div class="button-container">
          <md-button class="md-raised md-primary" @click="generatePassword">Generate Again</md-button>
          <md-button class="md-raised" v-clipboard:copy="password"  v-clipboard:success="onCopy"><md-icon>content_copy</md-icon> Copy</md-button>
        </div>
        <md-snackbar :md-position="vertical + ' ' + horizontal" ref="snackbar" :md-duration="duration">
        <span>Copied!</span>
        </md-snackbar>
      </div>
    </div>
    <div class="page-layout">
    <qrcode :value="password" :options="{ size: 200 }"></qrcode>
    </div>
    <div class="footer">
      <!-- Place this tag where you want the button to render. -->
      <div>
        <span class="md-caption">Client-side password generator.</span><br />
        <span class="md-caption">Donations are Always Welcome.<br />
          ETH: 0xca349aac7c3ddf94dd8ad5cad46039c44405f4cb <br />
          BTC: 1Fo2eEhw1zLkC8uWBqGR3ubBDYMimza1bk <br />
          BCC: 1Fo2eEhw1zLkC8uWBqGR3ubBDYMimza1bk <br />
          USDT: 1HcLTpyez6e9t5Qv7YJ3sFBAo1QPMKN6px <br />
        </span>
      <br />
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.css'
import VueClipboard from 'vue-clipboard2'
import VueQrcode from '@xkeshi/vue-qrcode'

Vue.use(VueMaterial)
Vue.use(VueClipboard)
Vue.component('qrcode', VueQrcode)

Vue.material.registerTheme('blue', {
  primary: 'blue',
  accent: 'red',
  warn: 'red'
})

Vue.material.setCurrentTheme('blue')

export default {
  name: 'app',
  data() {
    return {
      includeSymbols: false,
      includeNumbers: true,
      includeLowercaseCharacters: true,
      includeUppercaseCharacters: true,
      passwordLength: 30,
      password: '',
      vertical: 'bottom',
      horizontal: 'center',
      duration: 4000
    };
  },
  watch: {
    '$route'() {
      this.$nextTick(() => {
        window.Prism.highlightAll();
        this.$refs.pageContent.scrollTop = 0;
        this.showSidebar = false;
      });
    }
  },
  created() {
    this.generatePassword();
  },
  mounted() { },
  methods: {
    onCopy() {
      this.$refs.snackbar.open()
    },
    generatePassword() {
      const lowercaseCharacters = 'abcdefghijklmnopqrstuvwxyz';
      const uppercaseCharacters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
      const symbols = '@#$%[]{}!^';
      const numbers = '1234567890';
      const randoms = [];
      if (this.includeSymbols) {
        randoms.push(symbols);
      }
      if (this.includeNumbers) {
        randoms.push(numbers);
      }
      if (this.includeLowercaseCharacters) {
        randoms.push(lowercaseCharacters);
      }
      if (this.includeUppercaseCharacters) {
        randoms.push(uppercaseCharacters);
      }
      this.password = '';
      for (var i=0; i < this.passwordLength; i++) {
        const a = randoms[Math.floor(Math.random() * randoms.length)];
        const c = a.charAt(Math.floor(Math.random() * a.length));
        this.password += c;
      }
    }
  }
};
</script>
<style>
  .page-layout, .footer {
    padding-top: 30px;
    margin: 16px auto;
    display: flex;
    max-width: 600px;
  }
  .contaner {
    width: 100%;
  }
  .switch-container .md-switch {
    width: 100%;
  }
  .button-container {
    padding-top: 20px;
  }
</style>
