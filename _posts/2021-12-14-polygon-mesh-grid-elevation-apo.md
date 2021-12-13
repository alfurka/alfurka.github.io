---
layout: post
title: Elevation of a Polygon - A Python Script
subtitle: A Python script to generate mesh grid of a polygon
tags: [polygon, python, script]
image: /img/hoca.jpg
bigimg: /img/code.jpg
---

The purpose of this script is to generate a mesh grid within a Polygon. The coordinates of the mesh grid is then used to get elevation data from a public API: api.open-elevation.com. The current script can be easly adjust for other APIs. The output of the script are (1) the coordinates within the polygon and (2) elevations of the coordinates. Data can be used for various purposes. The script can be found in my repository: [PolygonElevations](https://github.com/alfurka/PolygonElevations). The script file is: `PolygonElevations.py`

**PolygonElevations.Client(polygon, resolution = 100, chunk_size = 50, sleep_time = 0)**:

- `polygon`: A shapely.geometry.Polygon object. 
- `resolution` (Integer): he number of grid points in one axes. The number increases at rate resolution^2. 
- `chunk_size`: the number of elevation points sent to open-elevation.com. Requests for large numbers could be rejected.
- `sleet_time` (seconds): If large number of points are required, Open-elevation can block user. sleep_time slows down the data requests. 

### Requirements


The required Python packages: Pandas, Numpy, Matplotlib, and Shapely.

```
pip install pandas numpy matplotlib shapely
```

### Initializing

Client must receive a Polygon shape via Polygon object from `shapely` package:

```Python
import pandas as pd
import matplotlib.pyplot as plt
from PolygonElevations import Client
from shapely.geometry import Polygon
```

Let's get a Polygon from Istanbul, Turkey:

```Python
istanbul = Polygon([(40.986044584788715, 28.935032042041136), 
                (41.0572627603291, 28.82871669300217), 
                (41.09379592092697, 29.005335537539224),
                (40.996329046194845, 29.095260376068758)])
```

I initialize my Client:

```Python
my_client = Client(istanbul, resolution = 100, chunk_size = 100)
```

### Polygon Shape

```Python
my_client.plot_polygon()
```
![](img/istanbul_poly.png)

`plot_polygon()` function can take arguments for [matplotlib.pyplot.plot](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html). For example:

```Python
my_client.plot_polygon(color='red')
```

![](img/istanbul_poly_red.png)

### Getting Elevation Data

`get_data()` function retrives elevation points for all coordinates. It returns the first five elements of data points ([pandas.DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)). The data is saved to `Client.data` object as [pandas.DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html).

```Python
my_client.get_data()
```

|   |  latitude | longitude | elevation |
|--:|----------:|----------:|----------:|
| 0 | 40.987133 | 28.933719 |         0 |
| 1 | 40.987133 | 28.936411 |         0 |
| 2 | 40.987133 | 28.939103 |         0 |
| 3 | 40.987133 | 28.941796 |         0 |
| 4 | 40.987133 | 28.944488 |         0 |

### Elevation Map

```Python
my_client.plot_elevation()
```

![](img/istanbul_elev.png)

`plot_elevation()` function can take arguments for [matplotlib.pyplot.scatter](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html). For example:

```Python
my_client.plot_elevation(marker = 's')
```

![](img/istanbul_elev_sq.png)
