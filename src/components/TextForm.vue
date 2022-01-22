<template>
  <form autocomplete="off" @submit.prevent="submitForm">
    <!-- <label for="speech">Company Name:</label><br /> -->
    <input
      type="text"
      id="company"
      name="company"
      placeholder="Company name..."
      class="form-control"
    />
    <!-- <label for="speech">Speech:</label><br /> -->
    <textarea
      type="text"
      id="speech"
      name="speech"
      rows="7"
      cols="120"
      placeholder="Enter speech text here..."
      class="form-control"
      :value="ttext"
    />
    <input type="submit" class="btn btn-dark w-100" />
    <small v-if="showFormMessage" id="emailHelp" class="form-text text-muted">{{
      formMessage
    }}</small>
  </form>
</template>

<script>
export default {
  props: {
    ttext: {
      required: false,
      type: String,
      default: "",
    },
  },

  data: () => ({
    showFormMessage: false,
    formMessage: "you are bad",
  }),

  methods: {
    submitForm(submitEvent) {
      fetch("https://welikethespeech.herokuapp.com/api/score-speech", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          company: submitEvent.target.elements.company.value,
          text: submitEvent.target.elements.speech.value,
        }),
      })
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          console.log(data);
          this.showFormMessage = true;
          this.emitter.emit("update_table", null);
        })
        .catch((err) => {
          console.error("Couldn't send post", err);
        });
    },
  },
};
</script>

<style scoped>
textarea#speech {
  width: 100%;
}

form * {
  margin-bottom: 0.5rem !important;
}
</style>
