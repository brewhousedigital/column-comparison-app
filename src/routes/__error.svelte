<script context="module">
    export function load({ error, status }) {
        return {
            props: {
                status: status,
                error: error.message
            }
        };
    }
</script>

<script>
    export let status;
    export let error;
</script>

<h1>Status: {status}</h1>
<h2>Error: {error}</h2>