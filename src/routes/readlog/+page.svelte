<script>
    let mode = $state(0);
    let rawLog = $state("");

    let readable = $state([]);

    import { onMount } from "svelte";

    function getJson() {
        readable = JSON.parse(rawLog);
        console.log(readable);
        mode = 1;
    }
</script>
<svelte:head>
    <title>Read Logs</title>
</svelte:head>
<style>
    #container {
        position: fixed;
        transform: translate(-50%, -50%);
        transition: top 0.5s ease-in-out;
        left: 50%;
        top: 50%;
        padding: 20px;
        background-color: rgb(136, 61, 33);
        box-shadow: 0px 0px 20px 10px rgba(127, 91, 24, 0.544);
        border-radius: 20px;
        width: 90%;
        height: 78.5%;
        z-index: 999;
    }

    .moment {
        color: white;
        padding: 10px;
        border-radius: 10px;
        margin: 10px;
        z-index: 1000;
        position: relative
    }
    .header {
        z-index: 1000;
        position: relative;
        margin-top: 50px;
    }

    form input[type="text"] {
        width: 100%;
        height: 200px;
    }
    form input[type="submit"] {
        transform: scale(1.5);
        display: block;
        margin: 0 auto;
        margin-top: 30px;
    }
</style>
<div style="top: 0; right: 0; left: 0; bottom: 0; background-color: black; z-index: 0; position: fixed;">h1</div>

{#if mode == 0}
<div id="container">
    
    <form onsubmit={(event) => {event.preventDefault(); getJson();}}>
        <input type="text" bind:value={rawLog}>
        <input type="submit" value="Submit">
    </form>

</div>
{:else}
    <div class="header">
        <h1>{(readable[readable.length-1].time - readable[0].time)/1000/60} minutes</h1>
    </div>
    {#each readable as x}
    <div class="moment">
        <h2>{x.log}</h2>
        <h3 style:text-align="left">{x.timeString}</h3>
        {#if x.log == "Game ended"}
        <p style:text-align="left">----------------------------------</p>
        {/if}
    </div>
    {/each}
{/if}