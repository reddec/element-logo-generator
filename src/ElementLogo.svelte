<script>
    import {afterUpdate, createEventDispatcher} from 'svelte';

    const dispatcher = createEventDispatcher();

    export let symbol = 'Fa';
    export let name = 'fluent-amqp-from-reddec-with-love';
    export let tags = ['go', 'CLI', 'lib'];
    export let background = '#800000';
    export let tile = '#a02c2c';
    export let text = '#ffffff';

    const width = 256;
    const height = 256;
    const radius = 16;
    const tileRadius = 4;
    const tilePad = 16;

    let svg;

    afterUpdate(() => {
        dispatcher('input', {svg: svg});
    });

    $: fontSize = (height / 1.6);
    $: fontTileSize = (fontSize / 5);
    $: textOffset = 20;
    $: tileWidth = (fontTileSize / 2) * 3 + 4;
    $: tileHeight = (fontTileSize + 4);
</script>
<svg xmlns="http://www.w3.org/2000/svg"
     bind:this={svg}
     style="dominant-baseline: hanging;"
     {width} {height} viewBox="0 0 {width} {height}"
     stroke-linecap="round" stroke-linejoin="round">
    <defs>
        <style type="text/css">@import url(https://fonts.googleapis.com/css?family=Ubuntu+Mono);</style>
    </defs>
    <rect {width} {height} fill="{background}" rx="{radius}" ry="{radius}"/>
    <text x="10" y="{textOffset}"
          text-anchor="start"
          fill="{text}"
          font-size="{fontSize}"
          font-style="normal"
          font-family="Ubuntu Mono"
    >{symbol.substr(0,2)}</text>
    {#each tags.filter((x)=>x.length > 0).slice(0, 3) as tag,index}
        <g transform="translate({width - fontTileSize - 32}, {20 + ((fontTileSize + tilePad) * index)})">
            <rect width="{tileWidth}"
                  height="{tileHeight}"
                  rx="{tileRadius}"
                  ry="{tileRadius}"
                  fill="{tile}"/>
            <text
                    x="{tileWidth / 2}"
                    y="{tileHeight/ 2}"
                    alignment-baseline="central"
                    text-anchor="middle"
                    fill="{text}"
                    font-size="{fontTileSize}"
                    font-style="normal"
                    font-family="Ubuntu Mono">{tag.substr(0, 3)}</text>
        </g>
    {/each}
    <text
            x="20"
            y="{height - 2 * fontTileSize}"
            alignment-baseline="hanging"
            text-anchor="start"
            fill="{text}"
            font-size="{fontTileSize}"
            font-style="normal"
            font-family="Ubuntu Mono"
    >
        {name.substr(0, 14)}
    </text>
</svg>