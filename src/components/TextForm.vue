<template>
    <form autocomplete="off" @submit.prevent="submitForm">
        <label for="speech">Company Name:</label><br />
        <input type="text" id="company" name="company" /><br />
        <label for="speech">Speech:</label><br />
        <textarea type="text" id="speech" name="speech" rows="10" cols="120" /><br />
        <input type="submit" />
    </form>
</template>

<script>
export default {
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
                    this.emitter.emit("update_table", null);
                })
                .catch((err) => {
                    console.error("Couldn't send post", err);
                });
        },
    },
};
</script>
