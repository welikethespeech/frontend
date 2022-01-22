<template>
  <div>
    <form autocomplete="off" @submit.prevent="submitForm">
      <input
        type="text"
        id="url"
        name="url"
        placeholder="URl to the website"
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
    formMessage: "you are bad",
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

          return res.json();
        })
        .then((data) => {
          console.log(data);
          this.showFormMessage = false;
          this.emitter.emit("update_table", null);

          this.$refs.textForm.setText(data.scraped);
        })
        .catch((err) => {
          this.websiteWait = false;

          this.showFormMessage = true;
          console.error("Couldn't send post", err);
        });
    },
  },
};
</script>

<style scoped>
form * {
  margin-bottom: 0.5rem !important;
}

span {
  margin-bottom: 0 !important;
}
</style>
