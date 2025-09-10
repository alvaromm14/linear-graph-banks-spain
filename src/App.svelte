<script>
  import data from "$data/data.js";
  import Line from "$components/Line.svelte";
  import Tooltip from "$components/Tooltip.svelte";
  import AxisX from "$components/AxisX.svelte";
  import AxisY from "$components/AxisY.svelte";
  import HoverEvents from "$components/HoverEvents.svelte";
  import { scaleLinear, scaleTime } from "d3-scale";
  import { timeParse, timeFormat } from "d3-time-format";
  import { line, curveBasis  } from "d3-shape";
  import { max, extent } from "d3-array";

  const margin = { top: 30, right: 20, bottom: 40, left: 50 };

  let width = 600;
  let height = 400;

  $: innerWidth = width - margin.left - margin.right;
  let innerHeight = height - margin.top - margin.bottom;

  const parseDate = timeParse("%Y-%m-%d");

  data.forEach((d) => {
    d.Fecha = parseDate(d.Fecha);
  });

  $: xScale = scaleTime()
    .domain(extent(data, (d) => d.Fecha))
    .range([0, innerWidth]);

  const yScale = scaleLinear()
    .domain([0, 75])
    .range([innerHeight, 0]);

  $: lineAsequibilidad = line()
    .curve(curveBasis)
    .x((d) => xScale(d.Fecha))
    .y((d) => yScale(d.Indice));

  let maxDate = data[data.length - 1].Fecha;

  let hoveredDate = maxDate;
</script>

<div class="main" bind:clientWidth={width}>
  <h2>Activos fuera de España</h2>
  <div class="chart-container">
    <svg {width} {height}>
      <g class="inner-chart" transform="translate({margin.left}, {margin.top})">
        <AxisX {xScale} height={innerHeight} width={innerWidth}/>
        <AxisY {yScale} width={innerWidth}/>
        <Line {xScale} {yScale} {data} {hoveredDate} {innerHeight} />
        <HoverEvents
          bind:hoveredDate
          {maxDate}
          {margin}
          {innerWidth}
          {innerHeight}
          {xScale}
        />
        <Tooltip {hoveredDate} {xScale} {yScale} {innerWidth} {data} />
      </g>
    </svg>
  </div>
</div>

<style>

  h2 {
    font-size: 15px;
    margin-bottom: 6px;
    font-weight: 400;
    font-style: italic;
    text-align: center;
    position: relative;
  }

  /* Estilos para dispositivos con pantallas más grandes (ordenadores) */
  @media (min-width: 768px) {
    .main {
      max-width: 700px;
              margin: 0 auto; /* centra horizontalmente */
    }
  }

  /* Estilos para dispositivos con pantallas más pequeñas (móviles) */
  @media (max-width: 767px) {
    .main {
      max-width: 100%;
        margin: 0 auto; /* centra horizontalmente */

    }
  }
  :global(.tick-text) {
    font-size: 12px;
    font-weight: 400;
    fill: hsla(212, 10%, 53%, 1);
    user-select: none;
  }

</style>
