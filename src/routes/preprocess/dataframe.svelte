<script>
    export let data;
</script>

<div class="flex flex-col mt-8">
    <div class="table-header">
        <table class="min-w-full">
            <thead>
                <tr>
                    {#if data.length > 0}
                    {#each Object.keys(data[0]) as key}
                    <th class="px-6 py-4 border-b-2 border-gray-300 text-center text-blue-500 tracking-wider">{key}</th>
                    {/each}
                    {/if}
                </tr>
            </thead>
        </table>
    </div>
    <div class="table-container">
        <table class="min-w-full">
            <tbody>
                {#each data as row, rowIndex}
                <tr key={rowIndex}>
                    {#each Object.values(row) as value}
                    <td class="px-6 py-3 text-center border-b border-gray-500">{value}</td>
                    {/each}
                </tr>
                {/each}
            </tbody>
        </table>
    </div>
</div>

<style>
    .table-container {
        display: block;
        max-height: 500px;
        overflow-y: auto;
    }
    .table-header, .table-container {
        width: 100%;
    }
    .table-header th {
        position: sticky;
        top: 0;
        background-color: white;
        z-index: 10;
    }
</style>