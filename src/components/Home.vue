<template>
    <TextForm />
    <br />
    <Table />
</template>

<script>
import Table from "./Table.vue";
import TextForm from "./TextForm.vue";

export default {
    components: {
        Table,
        TextForm,
    },
};
</script>
