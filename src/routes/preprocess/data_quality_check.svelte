<script>
    import Dataframe from "./dataframe.svelte";
    export let data, handleNext;
    import { onMount } from "svelte";
    import Spinner from "./spinner.svelte";

    let loading = false;
    let chartImage = '';

    async function get_chart() {
        loading = true;
        const response = await fetch('http://localhost:8000/get/chartData', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ data: data })
        });

        const responseData = await response.json();
        chartImage = 'data:image/png;base64,' + responseData.chart;
        loading = false;
    }

    onMount(() => {
        get_chart();
    });
</script>

<Dataframe data={data} />
{#if loading}
    <div class="flex flex-col items-center justify-center mt-12">
        <Spinner />
    </div>
{:else}
<div>
    <button class="px-4 py-2 text-white bg-blue-500 rounded-md hover:bg-blue-600"
        on:click={handleNext}>
        Next
    </button>
</div>
<div class="flex flex-col mt-8 max-h-96">
    <img src={chartImage} alt="Chart" />
</div>
{/if}