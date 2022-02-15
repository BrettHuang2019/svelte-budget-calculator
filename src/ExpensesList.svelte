<script>
    import SectionTitle from './Title.svelte'
    import Expense from './Expense.svelte'
    export let expenses = []
</script>

<section>
    <SectionTitle title="expense list" />
    <ul>
        {#each expenses as expense}
            <Expense {...expense} />
        {:else}
            <h2>currently there no expense</h2>
        {/each}
    </ul>
</section>

<style>
    h2 {
        text-transform: capitalize;
    }
</style>
