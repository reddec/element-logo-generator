<script>
    import ElementLogo from "./ElementLogo.svelte";
    import Picker from "./Picker.svelte";
    import saveAs from 'file-saver';

    let symbol = 'Fa';
    let name = 'fluent-amqp';
    let backgroundColor = '#400000';
    let tagColor = '#a02c2c';
    let textColor = '#ffffff';
    $: tags = rawTags.split(',').map((x) => x.trim());

    let rawTags = 'go, CLI, lib';

    let element;

    function updated(event) {
        element = event.detail;
    }

    function saveAsSVG() {
        let svgData = new XMLSerializer().serializeToString(element.svg);
        let svgBlob = new Blob([svgData], {type: "image/svg+xml;charset=utf-8"});
        saveAs(svgBlob, name + '.svg');
    }

    function saveAsPNG() {
        let svg = element.svg;
        let svgData = new XMLSerializer().serializeToString(svg);
        let svgBlob = new Blob([svgData], {type: "image/svg+xml;charset=utf-8"});
        let width = svg.width.baseVal.value,
            height = svg.height.baseVal.value,
            canvas = document.createElement("canvas"),
            context = canvas.getContext("2d"),
            exportFileName = name + '.png';
        canvas.width = width;
        canvas.height = height;
        let image = new Image(width, height);
        image.onload = () => {
            context.drawImage(image, 0, 0, width, height);
            canvas.toBlob((blob) => {
                saveAs(blob, exportFileName);
            });
        };
        image.src = URL.createObjectURL(svgBlob);
    }

</script>
<div style="display: flex; flex-wrap: wrap; ">
    <div style="margin: 0.2em">
        <form class="uniform">
            <label style="display: block">
                Two-symbol name
            </label>
            <input bind:value={symbol}/>
            <label  style="display: block">
                Element name
            </label>
            <input bind:value={name} maxlength="14"/>
            <label  style="display: block">
                Tags (comma separated)
            </label>
            <input bind:value={rawTags}/>
            <label  style="display: block">
                Background color
            </label>
            <input bind:value={backgroundColor}/>
            <Picker bind:value="{backgroundColor}"/>

            <label  style="display: block">
                Tag color
            </label>
            <input bind:value={tagColor} maxlength="14"/>
            <Picker bind:value="{tagColor}"/>

            <label  style="display: block">
                Text color
            </label>
            <input bind:value={textColor}/>
            <Picker bind:value="{textColor}"/>
        </form>
    </div>
    <div style="margin: 0.2em">
        <ElementLogo {symbol} {name} {tags}
                     on:input="{updated}"
                     text="{textColor}"
                     background="{backgroundColor}"
                     tile="{tagColor}"/>
        <div style="text-align: center">
            <small><a target="_blank" href="https://reddec.net/demo/element-logo-generator/">by RedDec</a></small>
        </div>
        <div style="margin-top: 1em; text-align: center">
            <button on:click={saveAsSVG}>save as SVG</button>
            <button on:click={saveAsPNG}>save as PNG</button>
        </div>
    </div>
    <div>
        <h2>Code for embedding</h2>
        <pre>
&lt;element-logo symbol="{symbol}" name="{name}" tags="{tags.join(',')}" background="{backgroundColor}" tile="{tagColor}"&gt;
  &lt;script defer src="https://cdn.jsdelivr.net/gh/reddec/element-logo-generator@1/dist/element-logo.min.js"&gt;&lt;/script&gt;
&lt;/element-logo&gt;
        </pre>
    </div>
</div>
<style>
    input {
        padding: 0.5em;
        margin: 0.5em;
        border: none;
        background-color: #dedede;
        border-radius: 8px;
    }
    button {
        padding: 0.5em;
        margin: 0.5em;
        border: none;
        background-color: #dedede;
        border-radius: 8px;
    }
</style>