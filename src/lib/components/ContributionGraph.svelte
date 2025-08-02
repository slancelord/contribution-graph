<script lang="ts">
    import { onMount } from "svelte"
    import axios from "axios"
    import ContributionCell from "./ContributionCell.svelte"
    import { subDays, format, endOfWeek } from "date-fns"

    const totalWeeks = 51
    const currentWeekEnd = endOfWeek(new Date(), { weekStartsOn: 1 })

    let contributions: Map<string, number> = $state(new Map())

    onMount(async () => {
        const { data } = await axios.get("https://dpg.gg/test/calendar.json")
        contributions = new Map(Object.entries(data))
    })

    function getContributionFromCell(row: number, column: number) {
        const daysAgo = (totalWeeks - 1 - column) * 7 + (7 - 1 - row)

        const date = subDays(currentWeekEnd, daysAgo)
        const stringDate = format(date, 'yyyy-MM-dd')

        const contribution = contributions.get(stringDate)

        return {
            date: stringDate,
            count: contribution ?? 0
        }
    }
</script>

<div class="wrapper">
    {#each { length: totalWeeks }, column}
        {#each { length: 7 }, row}
            <ContributionCell
                contribution={getContributionFromCell(row, column)}
            />
    {/each}
  {/each}
</div>

<style>
    .wrapper {
        display: grid;
        grid-template-columns: repeat(51, 15px);
        grid-template-rows: repeat(7, 15px);
        grid-auto-flow: column;
        gap: 2px; 
        width: fit-content;
    }
</style>