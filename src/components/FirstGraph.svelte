<script lang="ts">
    import { onMount } from "svelte";

    let fetchedData: any[] = [];

    function capitalizarPalavras(texto) {
        return texto
            .split(" ")
            .map(word => word.charAt(0).toUpperCase() + word.slice(1))
            .join(" ");
    }

    async function get_request_to_api() {
        const response = await fetch("http://localhost:8000/serie_a_table");

        try {
            fetchedData = await response.json();
            console.log(fetchedData)
        } catch (error) {
            console.error("Erro ao analisar JSON:", error);
        }
    }

    onMount(get_request_to_api)

</script>
<html>
<div class="container-main">
    <h2>Times:</h2>
    <div class="main-table">
        {#if fetchedData}
        <table>
            <thead>
                <tr>
                    {#each Object.keys(fetchedData?.[0] ?? {}) as head}
                        <th>{capitalizarPalavras(head.toString().replace("_", " "))}</th>
                    {/each}
                </tr>
            </thead>
            <tbody>
            {#each fetchedData as item}
                <tr>
                    {#each Object.values(item) as value}
                        <td>{value}</td>
                    {/each}
                </tr>
            {/each}
            </tbody>
        </table>
        {:else}
        <p>Loading...</p>
        {/if}
    </div>
</div>
</html>

<style>

table {
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    margin: auto;
    border-collapse: collapse;
    border: 1px solid black;
    text-align: center;
    vertical-align: middle;
}

thead th {
    background-color: black;
    color: white;
    margin: 13px;
    padding: 5px 13px;
    font-family: "Poppins", sans-serif;
}

tbody tr {
    padding: 2px 5px;
    color: black;
    font-family: "Poppins", sans-serif;
}

tbody tr:nth-child(odd) {
    background-color: #d9d6d6;
}

tbody tr:nth-child(even) {
    background-color: #b4b3b4;
}



</style>