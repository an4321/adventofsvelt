<script lang="ts">
    let error = null;
    let isLoading = $state(true);

    let data: any = $state([]);
    let selected: { name: string; weight: number }[] = $state([]);

    let total = $state(0);
    let safe = $state(true);

    async function fetchData() {
        try {
            const response = await fetch(
                "https://advent.sveltesociety.dev/data/2023/day-three.json",
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

    function toggle(name: string, weight: number) {
        const index = selected.findIndex((item) => item.name === name);

        if (index !== -1) {
            selected.splice(index, 1);
        } else {
            selected.push({ name, weight });
        }
        calc_total();
    }

    function calc_total() {
        total = Number(
            selected.reduce((acc, item) => acc + item.weight, 0).toFixed(2),
        );
        if (total >= 100) {
            safe = false;
        } else {
            safe = true
        }
    }
</script>

<h2>SLB™</h2>

{#if isLoading}
    <p>Loading...</p>
{/if}

{#if error}
    <p style="color: red;">{error}</p>
{/if}

<div class={safe ? "sleigh" : "unsafe"}>
    {#if selected.length == 0}
        Add Presents to sleigh..
    {:else}
        <b>Total: {total}</b>
        {#if !safe}
            <br />
            <b>Maximum capacity exceeded.</b>
        {/if}
        <ul>
            {#each selected as child}
                <li>
                    {child.name} : {child.weight}
                    <button onclick={() => toggle(child.name, child.weight)}>
                        -
                    </button>
                </li>
            {/each}
        </ul>
    {/if}
</div>

{#if data.length > 0}
    <ul>
        {#each data as child}
            {#if !selected.some((selectedItem) => selectedItem.name === child.name)}
                <li>
                    {child.name} : {child.weight}
                    <button onclick={() => toggle(child.name, child.weight)}
                        >+</button
                    >
                </li>
            {/if}
        {/each}
    </ul>
{/if}

<style>
    .sleigh {
        padding: 12px;
        background-color: #cccccc;
    }
    .unsafe {
        padding: 12px;
        background-color: #c22751;
    }
</style>
