<script lang="ts">
    import { page } from '$app/stores';
    let { children } = $props();
</script>

<div class="main">
    {#if $page.url.pathname === '/'}
        {@render children?.()}
    {:else}
        <h1>Day {$page.url.pathname.substring(1)}</h1>
        {@render children?.()}
    {/if}
</div>

<style>
.main {
    font-size: 1.2rem;
    max-width: 900px;
    margin-left: auto;
    margin-right: auto;
    padding-top: 24px;
}
</style>
