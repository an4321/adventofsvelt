<script lang="ts">
    let total = $state(0)
    let mood = $state("🙂")
</script>

<h3>Cookie Count: {total}</h3>

<button onclick={() => total ++}> 🍪 </button>
<button onclick={() => total --}> ➖ </button>
<button onclick={() => total = 0 }> 🔄 </button>


<h3>Santa’s mood: {mood}</h3>
<button onclick={() => mood = "🙂" }>🙂</button>
<button onclick={() => mood = "😄" }>😄</button>
<button onclick={() => mood = "😁" }>😁</button>

<style>
button {
    padding: 10px;
}
</style>
