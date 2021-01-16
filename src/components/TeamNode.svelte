<script>
    import * as d3 from "d3";
    export let data;
    export let year;
    export let colors;
    export let dimensions;
    export let team;


    let svg;

    const moneyFormat = d3.format(".4s")

    $: tree = function () {
        let root = d3
            .hierarchy({
                name: "emojis",
                children: data,
            })
            .sum((d) => d[year])
            .sort((a, b) => b[year] - a[year]);

        const treemap = d3
            .treemap()
            .size([dimensions.width, dimensions.height])
            .paddingInner(function(d){console.log(d); return 20})
            .paddingOuter(20)
            .tile(d3.treemapSlice)
            .round(false);

        return treemap(root);
    };
</script>
<h4>
    {team}
    { moneyFormat(d3.sum(data, d=> d[year]))}
</h4>
<svg width={dimensions.width} height={dimensions.height} bind:this={svg}>
    {#each tree().leaves() as d, index (d)}
    {#if d.value}
        <g transform={`translate(${d.x0}, ${d.y0})`}>
            <rect
                fill-opacity="1"
                width={d.x1 - d.x0}
                height={(d.y1 - d.y0) / 2}
                fill={colors.secondaryColor}
                />
            <rect
                y={d.y1/2 - d.y0/2}
                fill-opacity="1"
                width={d.x1 - d.x0}
                height={d.y1/2 - d.y0/2}
                fill={colors.mainColor }
                />
            <clipPath id={d.data.Player.replace(' ', '_')} />

            <text
                class='player-name'
                fill={'black'}
                clip-path={d.data.Player.replace(' ', '_')}
                
                dy="-3"
                dx="5">
                {d.data.Player + ' ' + moneyFormat(d.value)}
            </text>
        </g>
   {/if}
    {/each}
</svg>

<style>
    .player-name {
        
    }

</style>