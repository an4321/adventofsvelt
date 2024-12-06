<script lang="ts">
    let error = null;
    let isLoading = $state(true);

    let data: any = $state([]);
    let log: any = $state([]);
    let average = $state(0);

    async function fetchData() {
        try {
            const response = await fetch(
                "https://advent.sveltesociety.dev/data/2023/day-four.json",
            );
            if (!response.ok) {
                throw new Error(response.statusText);
            }
            data = await response.json();
        } catch (error: any) {
            error = error.message;
        } finally {
            isLoading = false;
        }

        log = [...log, data.heartRate];
    }
    // function getAverate() {
    //     let total: number;
    //     for (let i = 0; i < log.length; i++) {
    //         total += log[i];
    //     }
    // }

    fetchData();
    setInterval(() => {
        fetchData();
        // average = (log.reduce((a: number, b: number) => a + b, 0) / log.length)
        average = Math.round((log.reduce((a: number, b: number) => a + b, 0) / log.length) * 100) / 100;
    }, 1000); // Every Second
</script>

{#if isLoading}
    <p>Loading...</p>
{/if}

{#if error}
    <p style="color: red;">{error}</p>
{/if}

<h2>Live: {data.heartRate}</h2>
<h2>Average: {average}</h2>

<div class="log">
    <h3>Log</h3>
    <ul>
        {#each log as i}
            <li>{i}</li>
        {/each}
    </ul>
</div>

<style>
</style>
