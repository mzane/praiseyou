<template>
  <PraiseCreate v-if="isViewCreate" @showpraiseview="showPraiseView" />
  <PraiseView
    v-if="isViewView"
    @showpraisecreate="showPraiseCreate"
    :praiseMessage="praiseMessage"
    :praiseMessageEncoded="praiseMessageEncoded"
    :isPreview="isPreview"
  />

<!--  <footer class="page-footer">
    <p>praiseyou.online proudly powered by <a href="https://matthiaskrumm.name" target="_blank">Matthias Krumm</a></p>
  </footer>-->
</template>

<script>
import PraiseCreateVue from "./components/PraiseCreate.vue";
import PraiseViewVue from "./components/PraiseView.vue";

import { DELIMITER } from "./shared";

const HASH_CHARACTER = "#";
const VIEWS = {
  CREATE: "create",
  CREATED: "created",
  VIEW: "view",
};

export default {
  name: "App",
  components: {
    PraiseCreate: PraiseCreateVue,
    PraiseView: PraiseViewVue,
  },
  data: () => ({
    view: VIEWS.CREATE,
    praiseMessage: {},
    praiseMessageEncoded: "",
    isPreview: false,
  }),
  mounted() {
    if (window.location.hash) {
      const hash = window.location.hash.substr(1); // Remove hash-character itself
      this.decodePraiseMessage(hash);
      this.isPreview = false;
      this.view = VIEWS.VIEW;
    }
  },
  computed: {
    isViewCreate() {
      return this.view === VIEWS.CREATE;
    },
    isViewCreated() {
      return this.view === VIEWS.CREATED;
    },
    isViewView() {
      return this.view === VIEWS.VIEW;
    },
  },
  methods: {
    showPraiseCreate() {
      this.view = VIEWS.CREATE;
    },
    showPraiseView(payload) {
      this.decodePraiseMessage(payload.hash);
      this.isPreview = true;
      this.view = VIEWS.VIEW;
    },
    decodePraiseMessage(encodedMessage) {
      this.praiseMessageEncoded = `${window.location.protocol}//${window.location.host}${HASH_CHARACTER}${encodedMessage}`;
      let decompressedString = '';
      try {
        decompressedString = atob(encodedMessage);
      } catch(error) {
        console && console.error(`Unable to use atob, because of: ${error}`);
      }
      const messageParts = decompressedString.split(DELIMITER);

      this.praiseMessage = {
        recipientName: messageParts[0],
        praiseText: messageParts[1],
        senderName: messageParts[2],
      };
    },
  },
};
</script>

<style>
/*
Colors:
https://color.adobe.com/de/High-Praise-color-theme-7867743/
*/

* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
*:before,
*:after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

html {
  background-color: #F0F4F4;
  color: #08040C;
  font-family: "Lobster", cursive;
  font-size: 2em;
  font-weight: normal;
  line-height: 1.5;
  text-align: center;
}

@media (max-width: 600px) {
  html {
    font-size: 1.5em;
  }
}

h1,
h2 {
  color: #08040C;
  font-weight: normal;
}
h1 {
  color: #8A1E66;
  text-shadow: 2px 2px 2px #515947;
}
h2 {
  margin-bottom: 10px;
  font-size: 1.1em;
}

.input,
.select,
.btn {
  background: none;
  border: 0 none;
  border-bottom: 2px solid #8A1E66;
  border-radius: 0;
  -webkit-box-shadow: none;
  box-shadow: none;
  color: #F0F4F4;
  font-family: "Lobster", cursive;
  font-size: inherit;
  font-weight: inherit;
  line-height: inherit;
  outline: 0 none;
  padding: 0 20px;
}
select option {
  color: #8A1E66;
}

.btn {
  background: #8A1E66;
  border-radius: 10px;
  border-width: 0;
  color: #F0F4F4;
  cursor: pointer;
  margin: 1em 0;
  padding: 10px 30px;
  -webkit-transition: all 0.3s;
  -moz-transition: all 0.3s;
  transition: all 0.3s;
  text-decoration: none;
}
.btn::after {
  content: "";
  position: absolute;
  z-index: -1;
  -webkit-transition: all 0.3s;
  -moz-transition: all 0.3s;
  transition: all 0.3s;
}
.btn:hover,
.btn:active {
  color: #515947;
  background: #fff;
}
.btn[disabled],
.btn[disabled]:hover,
.btn[disabled]:active {
  background: #515947;
  color: #A0A4A4;
  cursor: default;
}

.btn-second {
  background: #515947;
}

.header-page {
  margin-bottom: 1em;
}

.praise-card {
  background-color: #515947;
  color: #F0F4F4;
  border-radius: 20px;
  -webkit-box-shadow: 2px 2px 2px #515947;
  box-shadow: 2px 2px 2px #515947;
  height: auto;
  margin: 0 auto 1em;
  max-height: 480px;
  max-width: 800px;
  min-height: 180px;
  min-width: 300px;
  padding: 30px;
}

.page-footer {
  font-size: .7em;
  position: fixed;
  left: 0;
  bottom: 0;
  right: 0;
}
</style>
