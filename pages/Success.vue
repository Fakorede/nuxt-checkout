<template>
    <div class="container">
        <div class="py-5 text-center">
            <h2>Success</h2>
            <p class="lead">Your purchase was successful!</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Success",
        async mounted() {
            const {data} = await this.$axios.post(`orders/confirm`, {
                source: this.$route.query.source
            })
        }
    }
</script>