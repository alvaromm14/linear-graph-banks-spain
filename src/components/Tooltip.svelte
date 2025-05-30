<script>
    export let xScale;
    export let yScale;
    export let hoveredDate;
    export let data;
    export let innerWidth;

    const getYValue = (date) => data.filter((d) => d.Fecha >= date)[0]?.Indice;

    function clasificarEnTrimestre(date) {
        const año = date.getFullYear();
        const mes = date.getMonth();

        const trimestre = Math.floor(mes / 3) + 1;

        return ` ${año}, T${trimestre}`;
    }
</script>

<circle
    cx={xScale(hoveredDate)}
    cy={yScale(getYValue(hoveredDate))}
    r="5"
    fill="#f26c55"
    stroke="white"
    pointer-events="none"
/>

<text
    x={xScale(hoveredDate) + 55 > innerWidth
        ? xScale(hoveredDate) - 16
        : xScale(hoveredDate)}
    dx="9"
    y={yScale(getYValue(hoveredDate))}
    pointer-events="none"
    stroke="white"
    stroke-width="3"
    paint-order="stroke"
    text-anchor={xScale(hoveredDate) + 55 > innerWidth ? "end" : "start"}
    dominant-baseline="middle"
    font-weight="700"
    font-size="12px"
>
    <tspan class="dato">
        {getYValue(hoveredDate)
            .toFixed(1)
            .toString()
            .replace(".", ",")
            .replace(/\B(?=(\d{3})+(?!\d))/g, ".")}%
    </tspan>
    <tspan
        class="fecha"
        x={xScale(hoveredDate) + 55 > innerWidth
            ? xScale(hoveredDate) - 10
            : xScale(hoveredDate) + 8}
        dy="1.3em"
    >
        {clasificarEnTrimestre(hoveredDate)}
    </tspan>
</text>

<style>
    .dato {
        font-size: 0.9rem;
    }
    .fecha {
        fill: black;
        font-weight: 400;
        font-size: 0.8rem;
    }
</style>
