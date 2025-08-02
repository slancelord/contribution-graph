<script lang="ts">
    import { parseISO, format } from 'date-fns'
    import { ru } from 'date-fns/locale'
    import Tooltip from '../Tooltip.svelte'

    let { contribution }: {contribution: {date: string, count: number}} = $props()

    let formattedDate = $state(format(parseISO(contribution.date), "EEEE, MMMM dd, yyyy", { locale: ru }))
    let showTooltip = $state(false)
   
    let color = $derived.by(() => {
        if (contribution.count <= 0) return "#EDEDED"
        if (contribution.count <= 9) return "#ACD5F2"
        if (contribution.count <= 19) return "#7FA8C9"
        if (contribution.count <= 29) return "#527BA0"
        return "#254E77"
    })
</script>

<button 
    class="cell"
    style="background-color: {color}"
    onfocus={() => showTooltip = true}
    onblur={() => showTooltip = false}
    aria-label={`Date: ${contribution.date}, contributions: ${contribution.count}`}
>

    {#if showTooltip}
        <Tooltip>
            <span slot="header">{contribution.count === 0 ? 'No' : contribution.count} contributions</span>
            <span slot="footer">{formattedDate}</span>
        </Tooltip>
    {/if}

</button>

<style> 
    .cell {
        position: relative;
        border: 1px solid transparent;
    }
    
    .cell:focus {
        outline: none;
        border-color: #000000E5;
    }

    .cell:hover {
        border-color: #00000080;
    }   
</style>