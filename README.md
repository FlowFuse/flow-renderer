# FlowFuse Flow Renderer

A library to render Node-RED flows in a web page.


## Installation

```bash
npm install @flowfuse/flow-renderer
```

## development

All client-side code is in the `index.js` file for easy inclusion in a web page.

Run `npm run demo` to test the flow renderer in a browser.

Run `npm run build` to build a minimised version of the flow renderer. This will output the file `index.min.js`.

IMPORTANT: The minified version of the flow renderer should be built before committing changes to the repository.

## Usage

Add the following script tag to your HTML file:

```html
<script type="module" src="public/flow-renderer/index.min.js"></script>
```
_or wherever the script is located in your project_

Next, add a container element to your HTML file and instantiate a `flowRenderer`.
NOTE: flow-renderer is an ES Module and requires a modern browser to run. Script tags must have the `type="module"` attribute.

By default, the flow renderer will render the flow with `gridLines`, `images`, `labels`, `zoom`, `autoZoom`, and `autoScroll` enabled.
`linkLines` are disabled by default.

To operate the zoom, use the mouse wheel + <kbd>Ctrl</kbd> or <kbd>⌘</kbd> key.
To scroll the container vertically, use the mouse wheel without the <kbd>Shift</kbd> key.
To scroll the container horizontally, use the mouse wheel + <kbd>Shift</kbd> key.



### Basic example

```html
<div id="nr-flow-1" class="flow-renderer" style="height: 300px"></div>
```

```html
<script type="module">
  const renderer = new FlowRenderer()
  const container1 = document.getElementById('nr-flow-1');
  const flow = [{"id": "1001", "type": "inject", "x": 100, "y": 40, "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":300, "y": 40}]
  renderer.renderFlows(flow, { container: container1 })
</script>
```

### Inline Options example

Options can be set by data attributes `scope`, `grid-lines`, `zoom`, `images`, `link-lines`, `labels`, `auto-zoom`, `auto-scroll`.

NOTE: To SET and option, the data attribute can simply be present on the container element. To UNSET an option, the data attribute must be set to `false`.

```html
<div id="nr-flow-2" style="height: 300px" 
    data-scope="custom-css-scope"
    data-grid-lines
    data-zoom
    data-images
    data-link-lines="false"
    data-labels
    data-auto-zoom
    data-auto-scroll
></div>
```

```html
<script type="module">
  const renderer = new FlowRenderer()
  const container2 = document.getElementById('nr-flow-2');
  const flow = [{"id": "1001", "type": "inject", "x": 100, "y": 40, "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":300, "y": 40}]
  renderer.renderFlows(flow, { container: container2 })
</script>
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
    scope: 'custom-css-scope', // scope for CSS
    gridLines: true, // show gridLines
    images: true, // show images
    linkLines: false, // show link lines
    labels: true, // show labels
    zoom: true, // enable zoom within the container
    autoZoom: true, // auto zoom to fit the flow within the container upon rendering (best fit, limited to 20% min, 100% max zoom)
    autoScroll: true, // auto scroll the leftmost node to the left of the container and the topmost node to the top of the container upon rendering
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


### Basic example comparing to flows

```html
<div id="nr-flow-1" class="flow-renderer" style="height: 300px"></div>
```

```html
<script type="module">
  const renderer = new FlowRenderer()
  const container1 = document.getElementById('nr-flow-1');
  const flow1 = [{"id": "1001", "type": "inject", "x": 100, "y": 40, "z": "9999", "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":300, "y": 40, "z": "9999"}]
  const flow2 = [{"id": "1001", "type": "inject", "x": 120, "y": 40, "z": "9999", "wires": [["1002"]]}, {"id": "1002", "type": "debug", "x":120, "y": 80, "z": "9999"}]
  renderer.compare([flow1, flow2], { container: container1 })
</script>
```


## Acknowledgements

This project owes a huge thanks to Gerrit Riessen for his original works on [node-red-flowviewer](https://github.com/gorenje/node-red-flowviewer-js). It was this great contribution that started the ball rolling. Gerrit kindly allowed us relicense the parts we needed to use in this project.

## Limitations

* Only client-side rendering is currently supported
* Mobile pinch zoom is not yet implemented
* The flow renderer does not support the full range of contributed Node-RED nodes however they will render as a generic node type complete with the node's label.
* The flow renderer does not always render the flows and nodes exactly as they appear in the Node-RED editor. This is due in part to being a client-side render with no server-side component to provide full context and partly due to the current limitations of the renderer itself.
* The flow compare functionality is still in very early development and may not work as expected if the flows are too dissimilar or are not well formed exports from Node-RED.

## Release process

In this project, the [Release Please](https://github.com/googleapis/release-please) is used to automatically determine the next release version based on the commit messages in the codebase.

By using the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/), the project adheres to a standardized format for commit messages, which `Release Please` uses to determine whether the next release should be a major, minor, or patch release.

### Components

1. The `Prepare release` GitHub Action workflow:

    * A Release Please action that analyzes commit messages to determine the type of release required (major, minor, patch) based on the Conventional Commits specification
    * Creates a pre-release pull request with the proposed version bump and changelog
    * Once merged, automatically updates the version number in `package.json` and creates a new release on GitHub with the appropriate changelog

2. The `Lint Pull Request Title` GitHub Action workflow:

    * A workflow that runs on pull request creation and uses the `amannn/action-semantic-pull-request` action to validate that pull request titles follow the Conventional Commits format
    * Together with adjusted default merge commit message, this ensures that all commits merged into the main branch adhere to the expected format, allowing Release Please to function correctly

3. The `Release Published` GitHub Action workflow:

    * A workflow that runs when a new git tag in `v*.*.*` format is pushed and is responsible for publishing the new version of the package to the public npm registry using the `JS-DevTools/npm-publish` action

### Pull Request Title Format

The Conventional Commits preset expects pull request titles to be in the following format:

```
<type>(<scope>): <subject>
```

* Type: Describes the category of the commit. Examples include:
    * `feat`: A new feature (triggers a minor version bump).
    * `fix`: A bug fix (triggers a patch version bump).
    * `perf`: A code change that improves performance (triggers a patch version bump).
    * `refactor`: A code change that neither fixes a bug nor adds a feature (does not trigger a release unless it's accompanied by a BREAKING CHANGE).
    * `docs`: Documentation-only changes (does not trigger a release).
    * `chore`: Changes to the build process or auxiliary tools and libraries (does not trigger a release).
* Scope: An optional part that provides additional context about what was changed (e.g., module, component).
* Subject: A brief description of the changes.

### Handling Breaking Changes

To indicate a breaking change, the exclamation mark `!` should be used immediately after the type/scope:

* `feat!:`
* `fix!:`
* `refactor!:`

## License

Apache-2.0
