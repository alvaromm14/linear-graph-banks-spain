<script>
    export let yScale;
    export let width;

    let yTicks = [25, 50, 75];
</script>

{#each yTicks as tick}
    <line
        x1=-40
        x2={width}
        y1={yScale(tick)}
        y2={yScale(tick)}
        stroke="black"
        stroke-width=0.3px
    />
    <text class="tick-text" x=-40 y={yScale(tick) - 6}>{tick}%</text>
{/each}

