# FlowFuse Flow Renderer

A library to render Node-RED flows in a web page.


## Installation

```bash
npm install @flowfuse/flow-renderer
```

## development

All client-side code is in the `index.js` file for easy inclusion in a web page.

Run `npm run demo` to test the flow renderer in a browser.

## Usage

Add the following script tag to your HTML file:

```html
<script type="module" src="public/flow-renderer/index.js"></script>
```
_or wherever the script is located in your project_

Next, add a container element to your HTML file and call the `flowRenderer` function with the flow data and options.
NOTE: flow-renderer is an ES Module and requires a modern browser to run. Script tags must have the `type="module"` attribute.

By default, the flow renderer will render the flow with `gridlines`, `images`, `labels` and `zoom` options enabled.

To operate the zoom, use the mouse wheel + <kbd>Ctrl</kbd> key.
To scroll the container vertically, use the mouse wheel without the <kbd>Shift</kbd> key.
To scroll the container horizontally, use the mouse wheel + <kbd>Shift</kbd> key.



### Basic example

```html
<div id="nr-flow-1" class="flow-renderer" style="height: 300px"></div>
```

```javascript
const renderer = new FlowRenderer()
const container1 = document.getElementById('nr-flow-1');
const flow = [{"id": "1001", "type": "inject", "x": 100, "y": 40, "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":300, "y": 40}]
renderer.renderFlows(flow, { container: container1 })
```

### Inline Options example

Options can be set by data attributes `scope`, `grid-lines`, `zoom`, `images`, `link-lines`, `labels`

```html
<div id="nr-flow-2" style="height: 300px" data-scope="my-scope" data-grid-lines="true" data-zoom="true" data-images="true" data-link-lines="false" data-labels="true"></div>
```

```javascript
const renderer = new FlowRenderer()
const container2 = document.getElementById('nr-flow-2');
const flow = [{"id": "1001", "type": "inject", "x": 100, "y": 40, "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":300, "y": 40}]
renderer.renderFlows(flow, { container: container2 })
```


### Full Options example

```html
<div id="nr-flow-3" class="my-scope" style="height: 300px"></div>
```

```javascript
const renderer = new FlowRenderer()
const container3 = document.getElementById('nr-flow-3');
const flow = [{"id": "1001", "type": "inject", "x": 100, "y": 40, "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":300, "y": 40}]
renderer.renderFlows(flow, {
    container: container3,
    scope: 'my-scope', // scope for CSS
    gridlines: true, // show gridlines
    images: true, // show images
    linkLines: false, // show link lines
    labels: true, // show labels
    zoom: true, // enable zoom within the container
    flowId: undefined // Id of flow to display
})
```

### Vue example

```html
    <div id="app">
        <div ref="f1" class="flow-renderer"></div>
    </div>

    <script type="module">
        import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'
        import FlowRenderer from './index.js'
        createApp({
            mounted() {
                const flow = [{"id": "1001", "type": "inject", "x": 100, "y": 40, "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":300, "y": 40}]
                const renderer = new FlowRenderer()
                renderer.renderFlows(flow, { container: this.$refs.f1 })
            }
        }).mount('#app')
    </script>
```

## Acknowledgements

This project owes a huge thanks to Gerrit Riessen for his original works on [node-red-flowviewer](https://github.com/gorenje/node-red-flowviewer-js). It was this great contribution that started the ball rolling. Gerrit kindly allowed us relicense the parts we needed to use in this project.

## Limitations

* Only client-side rendering is currently supported
* Mobile pinch zoom is not yet implemented
* The flow renderer does not support the full range of contributed Node-RED nodes however they will render as a generic node type complete with the node's label.
* The flow renderer does not always render the flows and nodes exactly as they appear in the Node-RED editor. This is due in part to being a client-side render with no server-side component to provide full context and partly due to the current limitations of the renderer itself.

## Versioning

While the API is in development, the version number of this package will remain at `0.x.y`.
`x` will be incremented for breaking changes, `y` for new features and patches.
Once the API is stable, the version number will be updated to 1.0.0 and full SemVer rules will be applied.

## License

Apache-2.0
