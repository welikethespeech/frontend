<template>
  <div>
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
      />

      <button v-if="submitWait" class="btn btn-dark w-100" disabled>
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
    <Results :data="resultsData" />
  </div>
</template>

<script>
import Results from "./Results.vue";

export default {
  components: {
    Results,
  },

  data: () => ({
    showFormMessage: false,
    formMessage: "error",
    submitWait: false,
    message: "",
    resultsData: null,
  }),

  methods: {
    setText(text) {
      this.$el.querySelector("#speech").value = text;
    },

    setErrorText(text) {
      this.showFormMessage = true;
      this.formMessage = text;
    },

    removeErrorText() {
      this.showFormMessage = false;
    },

    submitForm(submitEvent) {
      this.submitWait = true;

      fetch("https://welikethespeech.herokuapp.com/api/score-speech", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          company: submitEvent.target.elements.company.value,
          text: submitEvent.target.elements.speech.value,
        }),
      })
        .then((res) => {
          this.submitWait = false;

          if (res.status == 200) {
            return res.json();
          } else {
            res.json().then((data) => {
              this.showFormMessage = true;
              this.formMessage = data.message;
            });
          }
        })
        .then((data) => {
          console.log(data);
          this.emitter.emit("update_table", null);
          this.showFormMessage = false;
          this.resultsData = data;
        })
        .catch((err) => {
          this.submitWait = false;

          this.showFormMessage = true;
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
  margin-bottom: 0.5rem;
}

span {
  margin-bottom: 0 !important;
}
</style>
