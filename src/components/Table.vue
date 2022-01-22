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
            let fetchResult = [];
            fetch("https://welikethespeech.herokuapp.com/api/rankings", {
                method: "GET",
            })
                .then((res) => {
                    fetchResult = res;
                })
                .catch((err) => {
                    console.error("Couldn't send get", err);
                });
            return fetchResult;
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
