<template>
  <section class="praise-create">
    <form action="#" method="post">
      <header class="header-page">
        <h1>Praise someone!</h1>
      </header>

      <h2>Fill out praise card:</h2>
      <div class="praise-card">
        <div class="praise-who">
          <label>
            <input
              type="text"
              name="praise-who"
              ref="praise-who"
              class="input"
              maxlength="20"
              placeholder="Praised one's name"
              autocomplete="off"
              v-model="recipientName"
            />
          </label>
        </div>

        <div class="praise-text">
          <label>
            <input
              type="text"
              name="praise-text-long"
              ref="praise-text-long"
              class="input"
              maxlength="80"
              placeholder="Praise text"
              autocomplete="off"
              v-model="praiseTextLong"
            />
            <!--your are
            <select name="praise-text" class="select" v-model="praiseText">
              <option value="awesome">awesome</option>
              <option value="fabulous">fabulous</option>
              <option value="great">great</option>
            </select>
            <span>!</span>-->
          </label>
        </div>

        <div class="praise-from">
          <label>
            <input
              type="text"
              name="praise-from"
              ref="praise-from"
              class="input"
              placeholder="Your name"
              maxlength="20"
              autocomplete="off"
              v-model="senderName"
            />
          </label>
        </div>
      </div>

      <div class="praise-output">
        <button
          type="submit"
          class="btn"
          @click.prevent="createPraiseHandler"
        >
          Create praise card!
        </button>
        <!--<h2>
          2. Send this link to your friend via e-mail, messenger or whatever:
        </h2>
        <span id="praise-shorturl" class="out">
          https://praiseyou.online/#!<span v-text="praiseMessageEncoded"></span>
        </span>-->
        <!--<p style="margin-top: 0; font-size: 0.6em;">(click to copy to clipboard)</p>-->
      </div>
    </form>
  </section>
</template>

<script>
import { DELIMITER } from "../shared";

export default {
  name: "PraiseCreate",
  data: () => ({
    recipientName: "",
    praiseText: "awesome",
    praiseTextLong: "",
    senderName: "",
  }),
  computed: {
    /*enableCreateButton() {
      return !(this.recipientName && this.praiseTextLong && this.senderName);
    },*/
    praiseMessageEncoded() {
      return btoa(
        `${this.recipientName}${DELIMITER}${this.praiseTextLong}${DELIMITER}${this.senderName}`
      );
    },
  },
  methods: {
    createPraiseHandler() {
      if (!this.recipientName) {
        this.$refs['praise-who'].focus();
        return;
      } else if (!this.praiseTextLong) {
        this.$refs['praise-text-long'].focus();
        return;
      } else if (!this.senderName) {
        this.$refs['praise-from'].focus();
        return;
      }

      window.location.hash = this.praiseMessageEncoded;
      this.$emit("showpraiseview", { hash: this.praiseMessageEncoded });
    },
  },
};
</script>

<style scoped>
.praise-who {
  margin: 20px;
}

.praise-who input {
  width: 90%;
}

@media (max-width: 600px) {
  .praise-who input {
    width: 100%;
  }
}

.praise-text {
  margin: 20px;
}

.praise-text input {
  width: 90%;
}

@media (max-width: 600px) {
  .praise-text input {
    width: 100%;
  }
}

.praise-from {
  margin: 20px;
}

.praise-from input {
  width: 90%;
}

@media (max-width: 600px) {
  .praise-from input {
    width: 100%;
  }
}

.praise-output {
  color: #8A1E66;
  margin: 0 auto 1em;
}

.praise-output .out {
  border-width: 0;
  text-align: center;
  width: 100%;
  white-space: nowrap;
}
</style>
