<template>
  <table class="center">
    <thead>
      <tr>
        <th class="text-left">Company</th>
        <th class="text-left">Score</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="data in companies" :key="data">
        <td>{{ data.company }}</td>
        <td>
          {{ data.average_score.toFixed(2) }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  data: () => ({
    companies: [],
  }),

  mounted() {
    this.emitter.on("update_table", (data) => {
      this.getCompanies();
    });
  },

  created() {
    this.getCompanies();
  },

  methods: {
    getCompanies() {
      fetch("https://welikethespeech.herokuapp.com/api/rankings", {
        method: "GET",
      })
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          console.log(data);
          this.companies = data;
        })
        .catch((err) => {
          console.error("Couldn't send get", err);
        });
    },
  },
};
</script>

<style scoped>
.center {
  margin-left: auto;
  margin-right: auto;
}
</style>
