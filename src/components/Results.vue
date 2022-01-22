<template>
  <div v-if="data">
    <h3>Score: {{ data.score.toFixed(2) }}</h3>
    <table v-if="Object.keys(data.highlighted).length > 0" class="center">
      <thead>
        <tr>
          <th class="text-left">Sentence</th>
          <th class="text-left">Score</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(score, sentence) in data.highlighted" :key="sentence">
          <td>{{ sentence }}</td>
          <td>{{ score.toFixed(2) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: ["data"],

  mounted() {},

  created() {},

  methods: {},
};
</script>

<style scoped>
div {
  margin: 1rem 0;
}
</style>