<script>
    import DataQualityCheck from "./data_quality_check.svelte";
    import Dataframe from "./dataframe.svelte";
    import Spinner from "./spinner.svelte";
    let step = 0;
    function handleNext() {
        step++;
    }
    let upload_finished = false;
    let loading = false;
    let data = [];

    function handleFileUpload() {
        loading = true;
        const file = document.getElementById("file").files[0];
        const formData = new FormData();
        formData.append("file", file);
        fetch("http://localhost:8000/upload", {
            method: "POST",
            body: formData,
        })
        .then((res) => res.json())
        .then((res) => {
            data = res.data;
            loading = false;
            upload_finished = true;
        });
    }
</script>

<div class="flex flex-col pt-16 justify-center items-center">
    <div class="flex flex-row items-center space-x-8">
        <div class="flex flex-col items-center space-y-8">
            <img src={step == 0 ? "./round.png" : "./check.png"} alt="progress" class="w-6" />
            <span class={step == 0 ? "font-bold" : "text-gray-400"}>1. Data Upload</span>
        </div>
        <div class="flex flex-col items-center space-y-8">
            <img src={step <= 1 ? "./round.png" : "./check.png"} alt="progress" class="w-6" />
            <span class={step == 1 ? "font-bold" : step > 1 ? "text-gray-400" : ""}>2. Data Quality Check</span>
        </div>
        <div class="flex flex-col items-center space-y-8">
            <img src={step <= 2 ? "./round.png" : "./check.png"} alt="progress" class="w-6" />
            <span class={step == 2 ? "font-bold" : step > 2 ? "text-gray-400" : ""}>3. Data Preprocess</span>
        </div>
        <div class="flex flex-col items-center space-y-8">
            <img src={step <= 3 ? "./round.png" : "./check.png"} alt="progress" class="w-6" />
            <span class={step == 3 ? "font-bold" : step > 3 ? "text-gray-400" : ""}>4. Train ML model</span>
        </div>
        <div class="flex flex-col items-center space-y-8">
            <img src={step <= 4 ? "./round.png" : "./check.png"} alt="progress" class="w-6" />
            <span class={step == 4 ? "font-bold" : step > 4 ? "text-gray-400" : ""}>5. Check Score</span>
        </div>
    </div>
    <div class="flex flex-col mt-12 w-3/4">
    {#if step == 0}  
        <div class="flex flex-row space-x-12 justify-start content-start">
            {#if !upload_finished}
            <label for="file" class="px-4 py-2 text-white bg-blue-500 rounded-md hover:bg-blue-600 cursor-pointer">
                Upload CSV
            </label>
            <input type="file" accept=".csv" id="file" class="hidden" on:change={handleFileUpload} />
            {/if}
            {#if upload_finished}
            <div class="px-4 py-2 text-white bg-green-800 rounded-md">
                Upload Success!
            </div>
            <button class="px-4 py-2 text-white bg-blue-500 rounded-md hover:bg-blue-600"
                on:click={handleNext}>
                Next
            </button>
            {/if}
        </div>
        {#if loading}
        <div class="flex flex-col justify-center items-center mt-12">
            <Spinner />
        </div>
        {/if}
        {#if upload_finished}
            <Dataframe {data} />
        {/if}
    {/if}
    {#if step == 1}
        <DataQualityCheck {handleNext} {data} />
    {/if}
    </div>
</div>