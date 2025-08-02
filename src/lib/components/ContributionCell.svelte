<script lang="ts">
    export let contribution: { date: string, count: number}
    export let count: number = 0

    function handlerClick() {
        console.log(contribution.date)
    }
</script>

<button 
    class="cell"
    on:click={handlerClick}
    aria-label={`Date: ${contribution.date}, contributions: ${count}`}
>

</button>

<style> 
    .cell {
        background-color: #EDEDED;
        border: none;
    }
    
    .cell:focus {
        outline: none;
        border: 1px solid #000000E5;
    }

    .cell:hover {
        border: 1px solid #00000080;
    }
</style>