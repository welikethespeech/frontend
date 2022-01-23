<template>
  <div>
    <form autocomplete="off" @submit.prevent="submitForm">
      <input
        type="text"
        id="url"
        name="url"
        placeholder="URL to the website"
        class="form-control"
      />
      <input type="submit" class="btn btn-dark w-100" value="Extract" />
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
    formMessage: "error",
    websiteWait: false,
  }),

  methods: {
    submitForm(submitEvent) {
      this.websiteWait = true;

      fetch("https://welikethespeech.herokuapp.com/api/websitescrape", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          url: submitEvent.target.elements.url.value,
        }),
      })
        .then((res) => {
          this.websiteWait = false;

          if (res.status == 200) {
            return res.json();
          } else {
            res.json().then((data) => {
              this.$refs.setTextForms(data.message);
            });
          }
        })
        .then((data) => {
          console.log(data);
          this.$refs.textForm.removeErrorText();

          this.emitter.emit("update_table", null);

          this.$refs.textForm.setText(data.scraped);
        })
        .catch((err) => {
          this.websiteWait = false;

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
form .btn {
  flex: 0;
}

span {
  margin-bottom: 0 !important;
}
</style>
