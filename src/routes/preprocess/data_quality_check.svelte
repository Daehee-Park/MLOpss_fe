<script>
    import Dataframe from "./dataframe.svelte";
    export let data, handleNext;
    import { onMount } from "svelte";
    import Spinner from "./spinner.svelte";

    let loading = false;
    let chartImages = '';

    async function get_chart() {
        loading = true;
        const response = await fetch('http://localhost:8000/get/chart', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ data: data })
        });
        const responseData = await response.json();
        chartImages = responseData.charts.map(chart => 'data:image/png;base64,' + chart);
        loading = false;
    }

    onMount(() => {
        get_chart();
    });
</script>
{#if !loading}
<div>
    <button class="px-4 py-2 text-white bg-blue-500 rounded-md hover:bg-blue-600"
        on:click={handleNext}>
        Next
    </button>
</div>
{/if}
<Dataframe data={data} />
{#if loading}
    <div class="flex flex-col items-center justify-center mt-12">
        <Spinner />
    </div>
{:else}
<div class="grid grid-cols-4 mt-12">
    {#each chartImages as chartImage}
    <div class="max-w-full overflow-hidden">
        <img src={chartImage} alt="Chart" style="width: 100%; height: auto;" />
    </div>
    {/each}
</div>
{/if}