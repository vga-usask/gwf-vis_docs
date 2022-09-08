# Built-in Plugins

## All current provided built-in plugins

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

## Appendix

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
    ...
    'props': {
        'layerName': 'Catchment',
        'dataSource': dataURL
    }
}
```
If we want to add a custom color scheme, we should add the `colorScheme`:
```py
{
    ...
    'props': {
        'layerName': 'Catchment',
        'dataSource': dataURL,
        'colorScheme': {
            '': {
                'type': 'custom',
                'scheme': ['blue', 'green', 'yellow', 'red']
            }
        }
    }
}
```
This would change the color scheme to a color gradient of `['blue', 'green', 'yellow', 'red']`. In this example, the key `''` stands for all variables. We could also change color schemes for different variables:
```py
{
    ...
    'props': {
        'layerName': 'Catchment',
        'dataSource': dataURL,
        'colorScheme': {
            '': {
                'type': 'predefined',
                'scheme': 'blue-red'
            },
            'scalarSWE': {
                'type': 'custom',
                'scheme': ['blue', 'green', 'yellow', 'red']
            }
        }
    }
}
```
Assuming `scalarSWE` is a variable name. In this case, `scalarSWE` variable would have the color scheme of a color gradient of `['blue', 'green', 'yellow', 'red']` and all other varibales would have the build-in `blue-red` color scheme.  
If you also uses a __legend__, you should pass the same `colorScheme` dict to `props` of the __legend__.

### Config style

To be added ...