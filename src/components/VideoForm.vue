<template>
  <div>
    <form autocomplete="off" @submit.prevent="submitForm">
      <input
        type="text"
        id="url"
        name="url"
        placeholder="URl to the video"
        class="form-control"
      />

      <button v-if="videoWait" class="btn btn-dark w-100" disabled>
        <span class="spinner-border spinner-border-sm p-b-0"></span>
      </button>

      <input v-else type="submit" class="btn btn-dark w-100" />

      <small
        v-if="showFormMessage"
        id="emailHelp"
        class="form-text text-muted"
        >{{ formMessage }}</small
      >
    </form>

    <TextForm ref="textForm" />
  </div>
</template>

<script>
import TextForm from "./TextForm.vue";

export default {
  components: {
    TextForm,
  },

  data: () => ({
    showFormMessage: false,
    formMessage: "you are bad",
    videoWait: false,
  }),

  methods: {
    submitForm(submitEvent) {
      this.videoWait = true;

      fetch("https://welikethespeech.herokuapp.com/api/transcribe", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          url: submitEvent.target.elements.url.value,
        }),
      })
        .then((res) => {
          this.videoWait = false;

          return res.json();
        })
        .then((data) => {
          console.log(data);
          this.showFormMessage = false;
          this.emitter.emit("update_table", null);
          this.$refs.textForm.setText(data.transcription);
        })
        .catch((err) => {
          this.videoWait = false;

          this.showFormMessage = true;
          console.error("Couldn't send post", err);
        });
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
}
form * {
  margin-bottom: 0.5rem !important;
}

form #url {
  margin-right: 0.5rem;
  flex: 1;
}
form input.btn {
  flex: 0;
}
span {
  margin-bottom: 0 !important;
}
</style>
