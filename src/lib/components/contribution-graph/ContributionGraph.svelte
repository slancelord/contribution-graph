<script lang="ts">
    import { onMount } from "svelte"
    import axios from "axios"
    import ContributionCell from "./ContributionCell.svelte"
    import { subDays, format, endOfWeek } from "date-fns"
    import { ru } from 'date-fns/locale'
    import ContributionLegend from "./ContributionLegend.svelte";

    const totalWeeks = 51
    const currentWeekEnd = endOfWeek(new Date(), { weekStartsOn: 1 })
    const days = new Map([
        [0, 'Пн'],
        [2, 'Ср'],
        [4, 'Пт'],
    ])

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

    function getMonthFromWeek(month: number) {
        const daysAgo = (12 - month) * 30

        const date = subDays(currentWeekEnd, daysAgo)
        return format(date, 'MMM', { locale: ru });
    }
</script>

<div>
  <div class="month-labels">
    {#each { length: 12 }, column}
      <div class="month-label">
        {getMonthFromWeek(column)}
      </div>
    {/each}
  </div>

  <div class="main-grid">
    <div class="day-labels">
      {#each { length: 7 }, row}
        <div class="day-label">
          {days.get(row)}
        </div>
      {/each}
    </div>

    <div class="cells-wrapper">
      {#each { length: totalWeeks }, column}
        {#each { length: 7 }, row}
          <ContributionCell
            contribution={getContributionFromCell(row, column)}
          />
        {/each}
      {/each}
    </div>
  </div>

  <ContributionLegend/>
</div>


<style>
    .month-labels {
        display: grid;
        justify-items: center;
        grid-template-columns: repeat(12, 72px);
        margin: 0px 0px 5px 8px;
        color: #959494;
    }

    .main-grid {
        display: grid;
        grid-template-columns: auto 1fr;
        gap: 5px;
    }

    .day-labels {
        display: grid;
        grid-template-rows: repeat(7, 15px);
        gap: 2px;
        color: #959494;
    }

    .day-label {
        height: 15px;
        line-height: 15px;
        font-size: 12px;
    }

    .cells-wrapper {
        display: grid;
        grid-template-columns: repeat(51, 15px);
        grid-template-rows: repeat(7, 15px);
        grid-auto-flow: column;
        gap: 2px;
        width: fit-content;
    }
</style>