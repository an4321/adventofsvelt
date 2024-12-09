<script lang="ts">
    import MyChart from "./myChart.svelte";
    let error = null;
    let isLoading = $state(true);

    let data: any = $state([]);

    async function fetchData() {
        try {
            const response = await fetch(
                "https://advent.sveltesociety.dev/data/2023/day-five.json",
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
    }

    fetchData();
</script>

{#if isLoading}
    <p>Loading...</p>
{:else if error}
    <p style="color: red;">{error}</p>
{:else}
    <MyChart {data} />
    <ol>
        {#each data as task}
            <li>
                {task.elf} : {task.minutesTaken}min
                {#if task.task == "WRAPPED_PRESENT"}
                    🎁
                {:else if task.task == "CREATED_TOY"}
                    🧸
                {/if}
                ({task.date})
            </li>
        {/each}
    </ol>
    <!-- {JSON.stringify(data)} -->

    <canvas id="chart"></canvas>
{/if}
