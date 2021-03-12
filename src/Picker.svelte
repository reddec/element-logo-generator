<svelte:options tag={null}/>
<script>
    import {HsvPicker} from 'svelte-color-picker';
    import {createEventDispatcher} from 'svelte';

    const dispatcher = createEventDispatcher();
    export let value = '';
    let opened = false;

    function channelToHex(value) {
        let hex = Number(value).toString(16);
        if (hex.length < 2) {
            hex = "0" + hex;
        }
        return hex;
    }

    function asRGB({r, g, b}) {
        const red = channelToHex(r);
        const green = channelToHex(g);
        const blue = channelToHex(b);
        return red + green + blue;
    }

    function onColor({detail}) {
        value = '#' + asRGB(detail);
    }
</script>
<div style="display: inline-block; width: 1em; height: 1em; border: #333333 solid 1px; background-color: {value}"
     on:click={()=> opened = !opened}>

</div>
{#if opened}
    <HsvPicker startColor="{value}" on:colorChange={onColor}/>
{/if}