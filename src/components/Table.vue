<template>
    <table class="center">
        <thead>
            <tr>
                <th class="text-left">Company</th>
                <th class="text-left">Score</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in companies" :key="item.name">
                <td>{{ item.name }}</td>
                <td>{{ item.score }}</td>
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
            this.updateCompanies();
        });
    },

    created() {
        this.updateCompanies();
    },

    watch: {
        companies(nextVal, preVal) {
            // do stuff if needs to
        },
    },

    methods: {
        updateCompanies() {
            this.companies = this.getCompanies();
        },
        getCompanies() {
            return [
                { name: "company 1", score: 0.99 },
                { name: "company 1", score: 0.35 },
                { name: "company 2", score: 0.69 },
                { name: "company 3", score: 0.91 },
                { name: "company 4", score: 0.95 },
            ];
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
