<script lang="ts">
    import Tooltip from "../Tooltip.svelte";

	const levels = [
		{ label: "No contributions", color: "#EDEDED" },
		{ label: "1-9 contributions", color: "#ACD5F2" },
		{ label: "10-19 contributions", color: "#7FA8C9" },
		{ label: "20-29 contributions", color: "#527BA0" },
		{ label: "30+ contributions", color: "#254E77" }
	];

    let showTooltip: number | null = null 
</script>

<div class="legend">
	<span class="label">Меньше</span>
	{#each levels as level, i}
		<button 
            class="legend-box"
            style="background-color: {level.color}"
            onfocus={() => showTooltip = i}
            onblur={() => showTooltip = null}
        >
            {#if showTooltip === i}
                <Tooltip>
                    <span slot="header">{level.label}</span>
                </Tooltip>
            {/if}

        </button>
	{/each}
	<span class="label">Больше</span>
</div>

<style>
	.legend {
		display: flex;
		align-items: center;
		gap: 2px;
		margin: 12px 0 0 15px;
		font-size: 8px;
		color: #959494;
	}

	.legend-box {
        position: relative;
        border: 1px solid transparent;
		width: 15px;
		height: 15px;
	}

    .legend-box:focus {
        outline: none;
        border-color: #000000E5;
    }

    .legend-box:hover {
        border-color: #00000080;
    }   

	.label {
		margin: 0 4px;
	}
</style>