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

<main>
    <h1>Element logo generator</h1>
    <div style="text-align: center; padding-bottom: 3em">
        <i>
            <a href="https://reddec.net/about">by RedDec</a>
        </i>
    </div>
    <div style="display: flex; flex-wrap: wrap">
        <div style="padding: 1em">
            <form>
                <label>
                    Two-symbol name
                </label>
                <input bind:value={symbol}/>
                <label>
                    Element name
                </label>
                <input bind:value={name} maxlength="14"/>
                <label>
                    Tags (comma separated)
                </label>
                <input bind:value={rawTags}/>
            </form>
        </div>
        <div style="flex-grow: 1">
            <ElementLogo {symbol} {name} {tags}
                     on:input="{updated}"
                     text="{textColor}"
                     background="{backgroundColor}"
                     tile="{tagColor}"/>
            <div style="padding-top: 3em">
                <button on:click={saveAsSVG}>save as SVG</button>
                <button on:click={saveAsPNG}>save as PNG</button>
            </div>
        </div>
        <div>
            <form>
                <label>
                    Background color
                </label>
                <input bind:value={backgroundColor}/>
                <Picker color="{backgroundColor}"/>

                <label>
                    Tag color
                </label>
                <input bind:value={tagColor} maxlength="14"/>
                <Picker color="{tagColor}"/>

                <label>
                    Text color
                </label>
                <input bind:value={textColor}/>
                <Picker color="{textColor}"/>

            </form>
        </div>
    </div>

</main>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    label {
        font-weight: bold;
        display: block;
    }

    label > input {
        display: inline-block;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>