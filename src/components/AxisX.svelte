<script>
    export let height;
    export let xScale;
    import { timeFormat } from "d3-time-format";

    const formatYear = timeFormat("%Y");
    let xTicks = xScale.ticks(10).map(d => ({ value: d, label: formatYear(d) }));

</script>

<g class='axis x' transform="translate(0, {height})">
{#each [xTicks[0], xTicks[5], xTicks[9]] as { value, label }, index}
  <g class='tick' transform="translate({xScale(value)}, 0)">
    <line x1={0} x2={0} y1={0} y2={6} stroke="hsla(212, 10%, 53%, 1)" />
    <text class="tick-text"
      x={0}
      y={6}
      dy={10}
      dominant-baseline="middle"
      text-anchor="middle"
    >{label}</text>
  </g>
{/each}
  </g>