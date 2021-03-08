<template>
  <section class="praise-view">
    <div v-if="isPreview">Here is what the card looks likes:</div>

    <header class="header-page">
      <h1>You got praised!</h1>
    </header>

    <div class="praise-card">
      <div class="praise-who">
        <span v-text="praiseMessage.recipientName"></span>
      </div>

      <div class="praise-text">
        <span v-text="praiseMessage.praiseText"></span>
      </div>

      <div class="praise-from">
        <span v-text="praiseMessage.senderName"></span>
      </div>
    </div>

    <div v-if="isPreview" class="praise-preview-actions">
      <h2>What to do now:</h2>
      <p class="praise-preview-actions--desktop">
        <a href="#" class="btn" @click.prevent="copyToClipboard"
          >Copy URL to clipboard</a
        ><br /><br />and send it your friend!
      </p>
      <p class="praise-preview-actions--mobile">
        Use your browser's share-function to send this praise-card!
      </p>

      <div
          class="praise-url--hidden"
          ref="praise-url-div"
          v-text="praiseMessageEncoded"
      />
      <input
          type="text"
          class="praise-url&#45;&#45;hidden"
          ref="praise-url-textarea"
      />
    </div>

    <div class="praise-cta">
      <a
        href="#"
        class="btn btn-second"
        @click.prevent="showPraiseCreateHandler"
        >Now praise someone else!</a
      >
    </div>
  </section>
</template>

<script>
export default {
  name: "PraiseCreate",
  props: {
    praiseMessage: {
      type: Object,
      required: true,
    },
    praiseMessageEncoded: {
      type: String,
      required: true,
    },
    isPreview: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    showPraiseCreateHandler() {
      window.location.hash = "";
      this.$emit("showpraisecreate");
    },
    copyToClipboard() {
      const copyDiv = this.$refs["praise-url-div"];
      const copyTextarea = this.$refs["praise-url-textarea"];
      copyTextarea.value = copyDiv.innerText;
      copyTextarea.select();

      try {
        const successful = document.execCommand("copy");
        const msg = successful ? "successful" : "unsuccessful";
        console && console.log("Copying text command was " + msg);
      } catch (err) {
        console && console.error("Oops, unable to copy, because of:", err);
      }
    },
  },
};
</script>

<style scoped>
.praise-who {
  margin: 20px;
}

.praise-text {
  margin: 20px;
}

.praise-from {
  margin: 20px;
}

.praise-preview-actions {
  margin-top: 2em;
}

.praise-preview-actions p {
  line-height: 1;
}

.praise-preview-actions--desktop {
  display: block;
}
@media (max-width: 600px) {
  .praise-preview-actions--desktop {
    display: none;
  }
}

.praise-preview-actions--mobile {
  display: none;
}
@media (max-width: 600px) {
  .praise-preview-actions--mobile {
    display: block;
  }
}

.praise-cta {
  margin-top: 3em;
}

.praise-url--hidden {
  position: absolute;
  left: -1000em;
  top: -1000em;
}
</style>
