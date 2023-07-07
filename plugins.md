# Plugins
The system is designed to be plugin based, which gives you the flexibility of customization and adding potential features of your own.

## Built-in plugin list
We provided a set of built-in plugins, which are located in the `/public/plugins/default` directory. With the default config created by `gwfvis.load_vis_config()`, they are imported as below names.

### Data Fetcher
- _data-fetcher_

### Map Layer
- tile-layer
- geojson-layer

### Chart
- line-chart
- radar-chart

### Other
- user-selection
- metadata
- dimension-control
- variable-control
- legend

## Plugin containers
Plugins can be put into different containers.

### hidden
This container hides the plugin's user control and used to put plugins that does not have a user control (such as layer plugins). You can use `gwfvis.add_map_element()` to add plugins into it.

### main
Plugins in this container would be presented as overlays on the map. You can use `gwfvis.add_main_view_element()` to add plugins into it.

#### Container props
- `width` The width of the plugin to be displayed.

### sidebar
Plugins in this container would be presented inside the sidebar. You can use `gwfvis.add_sidebar_element()` to add plugins into it.

#### Container props
- `slot` If a value `"top"` is presented, the plugin would go to the very top part of the sidebar and would not be affected by the vertical scroll bar.

## Built-in plugin details

### _data-fetcher_
This one is included within the default config, you do not need to worry about it.

### tile-layer
It adds a __tile layer__ to the map. We provided a default setelitte layer config as a const for you to use like below:
```py 
gwfvis.add_map_element(vis_config, 'tile-layer', gwfvis.SATELITTE_LAYER_PROPS)
```

#### Props
- `layerName` _(required)_ The name of the layer, which would be added into the layer control.
- `urlTemplate` _(required)_ The url template. Check the "URL template" section from [here](https://leafletjs.com/reference.html#tilelayer) for more details.
- `options` The options (such as "attribution"). Check the "Options" section from [here](https://leafletjs.com/reference.html#tilelayer) for more details.
- `type` Default to `"base-layer"`. Check [here](#map-layer-types) for all potential values.
- `active` If `true`, the layer would be enabled by default. You can later toggle it from the __layer control__.


### geojson-layer
It adds a __GeoJSON layer__ (polygons, lines, or points) to the map.

#### Props
- `layerName` _(required)_ The name of the layer, which would be added into the layer control.
- `dataSource` _(required)_ The data source for the layer.
- `options` The options (such as "attribution"). Check the "Options" section from [here](https://leafletjs.com/reference.html#geojson) for more details.
- `type` Default to `"base-layer"`. Check [here](#map-layer-types) for all potential values.
- `active` If `true`, the layer would be enabled by default. You can later toggle it from the __layer control__.
- `variableName` If specified, the layer is bound to the specific variable.
- `dimensions` If specified, the layer is bound to the specific dimension selection. You should pass a [dimension selection](#dimension-dict) in.
- `colorScheme` If specified, the default color scheme is replaced. Check [here](#config-color-schemes) for more details.
- `usePushPins` If `true`, the points would be shown as push pins.

### line-chart
It draws a line chart across specific dimension values for the current selected location. Check [here](#advanced-interactions-for-the-line-chart) for some more interactions.

#### Props
- `dataSource` _(required)_ The data source for the chart.
- `dimension` _(required)_ The dimension name for the chart.
- `variableNames` If specified, the chart are bound to the specific variables; otherwise the chart are bound to the current selected variable from the __variable control__. You should pass a string list in.
- `locationLabelKey` If specified, the locations' labels, which are obtained from the specified field from metadata, would be shown instead of just showing location IDs. For example, if `locationLabelKey` is set as `my_label`, the location labels would be obtained from each location's `my_label` field of its metadata object.

### radar-chart
It draws a radar chart of multiple variables for current dimension and location selection.

#### Props
- `dataSource` _(required)_ The data source for the chart.
- `variableNames` If specified, the chart are bound to the specific variables; otherwise the chart are bound to the current selected variable from the __variable control__. You should pass a string list in.
- `dimensions` If specified, the chart is bound to the specific dimension selection. You should pass a [dimension selection](#dimension-dict) in.

### More
to be added...

## Implement your own plugins
To be added...

## Appendix

### Map Layer Types
- `"base-layer"`
- `"overlay"`

Only one _base layer_ can be presented at one time but multiple _overlay_ can be presented at the same time.

## Dimension Selection
Each dimension has a __name__ and a __value__. The __value__ should be an non-negative integer and sharts from 0. Assume we have dimension "time" and "depth", which have values of 10 and 5, the dimension selection should look like below:
```py
{
    'time': 10,
    'depth': 5
}
```

### Advanced interactions for the line chart

#### Checking specific values
When you hover your mouse to the lines, a tooltip of its specific value would be shown.

#### Axis zooming in/out and panning
When you move your mouse to axes of the line charts, you are able you drag to pan and scroll to zoom in/out. To prevent interrupt the normal scroll flow, to make a axis zooming, you need to hold `Ctrl` button and then scroll. If you are using a touch pad, you could just pinch over the axes to zoom in/out.

#### Jumping to specific dimension value
As the `x` axis stands for the dimension, we might find some interesting position across the axis. When you checking the specific values, you can click it, which would navigate the dimension to the selected value (same effect as you drag the bar of the __dimension control__).

#### Disable a line
When showing multiple lines, you can click a legend item to disable the coresponding line.


### Config color schemes
We provide the ability of change color scheme for the GeoJSON layer and the legend. To do so, we need to pass a `colorScheme` value to the config. 
Assume we have a config of GeoJSON layer like below:
```py
{
    # ...
    'props': {
        'layerName': 'Catchment',
        'dataSource': dataURL
    }
}
```
If we want to add a custom color scheme, we should add the `colorScheme`:
```py
{
    # ...
    'props': {
        'layerName': 'Catchment',
        'dataSource': dataURL,
        'colorScheme': {
            '': {
                'type': 'sequential',
                'scheme': ['blue', 'green', 'yellow', 'red']
            }
        }
    }
}
```
This would change the color scheme to a color gradient of `['blue', 'green', 'yellow', 'red']`. In this example, the key `''` stands for all variables. We could also change color schemes for different variables:
```py
{
    # ...
    'props': {
        'layerName': 'Catchment',
        'dataSource': dataURL,
        'colorScheme': {
            '': {
                'type': 'sequential',
                'scheme': 'interpolateOranges'
            },
            'scalarSWE': {
                'type': 'quantize',
                'scheme': 'schemeOranges[5]'
            }
        }
    }
}
```
Assuming `scalarSWE` is a variable name. In this case, `scalarSWE` variable would have the color scheme of a colors of 5 levels of orange color and all other varibales would have the gradient of orange color scheme.  
The avaiable types are `"sequential"`, `"quantize"`, and `""quantile`. For each type, pre-defined and custom scheme could be used. For custom schemes, a list of color strings should be passed, such as `['blue', 'green', 'yellow', 'red']`. For pre-defined schemes, please refer to color scheme definitions at [d3-scale-chromatic](https://github.com/d3/d3-scale-chromatic) for color scheme names. When using the pre-defined color schemes, you should pass it as a single sting, such as `"interpolateOranges"` or `"schemeOranges[5]"`. Note that for `"sequential"` type, you should use diverging color schemes; for `"quantize"` and `""quantile` types, you should use categorical color schemes.  
If you also uses a __legend__, you should pass the same `colorScheme` dict to `props` of the __legend__.  
Try `color_config.py` in [GWFVis](http://gwfvis.usask.ca/RiverFlow/).

### Config style

To be added ...