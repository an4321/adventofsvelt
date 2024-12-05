<script lang="ts">
    let data: any = [];
    let error = null;
    let isLoading = true;

    async function fetchData() {
        try {
            const response = await fetch("https://advent.sveltesociety.dev/data/2023/day-one.json");
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
{/if}

{#if error}
    <p style="color: red;">{error}</p>
{/if}

{#if data.length > 0}
    <ul>
        {#each data as children}
            {#if children.tally > 0}
                <li>{children.name} 🎁</li>
            {:else}
                <li>{children.name}</li>
            {/if}
        {/each}
    </ul>
{/if}

