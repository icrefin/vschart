## Why VSChart

  Recently I've been working on testing a couple of ideas on stock price analysis using Python.

  I need to visually verify the candlestick charts to see whether my program is working as expected.

  I tried a few plotting packages in Python, but none of them works as gracefully as TradingView.

  AND I do all the coding, tweaking, and debugging in Trae/VS Code.

  ------------------

  I decided to code this VS Code extension using the open source version "lightweight-charts" from TradingView, so that I can look at my codes and the chart at the same time in the same IDE window.

  The lightweight-charts provides a very nice and interactive chart with necessary basic functions, as well as an interface for user - customizable plugins.

  With that, I integrated a few useful plugins from lightweight-charts plugin examples that are available in its github repo.

  I also added a few new plugins that I find useful.

  Now, with this, I can happily work on my thing... Hope this helps you too.

## VS Code installation and usage
  Install the .vsix to your VS Code or Trae or Cursor or any other VS Code - based IDE.

  On Mac, press Command + Shift + p and then select "Open VSChart".

  For Windows, use Ctrl + Shift + p instead.

## Screenshot
![Screenshot 2025-08-15 at 19 36 48](https://github.com/user-attachments/assets/253742fd-2ab1-4dbb-bb49-9626d1a7d2f5)



# VSChart Python API Reference

## Table of Contents

- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Chart](#chart)
  - [Initialization](#initialization)
  - [Connection](#connection)
  - [Chart Creation](#chart-creation)
  - [Chart Appearance](#chart-appearance)
  - [Crosshair Configuration](#crosshair-configuration)
- [Series](#series)
  - [Candlestick Series](#candlestick-series)
  - [Line Series](#line-series)
  - [Volume Series](#volume-series)
- [Chart Elements](#chart-elements)
  - [Background Color](#background-color)
  - [Custom Marker](#custom-marker)
  - [Fibonacci](#fibonacci)
  - [Rectangle](#rectangle)
  - [Tooltip](#tooltip)
  - [Trend Line](#trend-line)
  - [Vertical Line](#vertical-line)
  - [Volume Profile](#volume-profile)
- [Constants](#constants)
  - [Line Style](#line-style)
  - [Line Type](#line-type)
  - [Price Line Style](#price-line-style)
  - [Crosshair Mode](#crosshair-mode)
  - [Marker Position](#marker-position)
  - [Marker Shape](#marker-shape)
  - [Color](#color)
  - [Size](#size)

## Installation

### From PyPI

```bash
pip install vschart-python
# if you want to taste what it does
# get tests/* from github and run to see how it works
cd tests
python test_all.py
```


## Basic Usage

Here's a simple example to create a chart with candlestick and volume data:

```python
from vschart import Chart
from vschart.constants import Color, LineStyle
import pandas as pd
import numpy as np

# Create chart instance
chart = Chart(host="localhost", port=8080)

# Connect to the server
if chart.connect():
    # Create chart
    chart.create()
    
    # Apply default options
    chart.apply_default_chart_options()
    
    # Add candlestick series
    candlestick_series = chart.add_candlestick_series(
        up_color=Color.TEAL_GREEN,
        down_color=Color.CORAL_RED
    )
    
    # Add volume series in a separate pane
    volume_series = chart.add_histogram_series(pane=1)
    
    # Set data
    candlestick_data = [
        {"time": 1672531200, "open": 100, "high": 105, "low": 98, "close": 103},
        {"time": 1672617600, "open": 103, "high": 107, "low": 101, "close": 104},
        # ... more data points
    ]
    
    volume_data = [
        {"time": 1672531200, "value": 1500},
        {"time": 1672617600, "value": 2100},
        # ... more data points
    ]
    
    candlestick_series.set_data(candlestick_data)
    volume_series.set_data(volume_data)
```

## Chart

### Initialization

```python
from vschart import Chart

# Default connection to localhost:8082
chart = Chart()

# Custom host and port
chart = Chart(host="127.0.0.1", port=8080)
```

### Connection

```python
# Connect to the VSCode plugin WebSocket server
success = chart.connect()

# Disconnect when done
chart.disconnect()
```

### Chart Creation

```python
# Create a chart with default options
chart.create()

# Create a chart with custom options
chart.create({
    "width": 800,
    "height": 600
})

# Apply default chart options
chart.apply_default_chart_options()
```

### Chart Appearance

```python
# Set separator color
chart.set_separator_color(Color.RED)

# Set separator hover color
chart.set_separator_hover_color(Color.RED)

# Enable/disable pane resizing
chart.set_enable_resize(True)

# Set chart width
chart.set_width(800)

# Set chart height
chart.set_height(600)

# Enable/disable auto size
chart.set_auto_size(True)

# Enable/disable default pane
chart.set_add_default_pane(True)

# Set background
chart.set_background(type="solid", color=Color.WHITE)

# Set text color
chart.set_text_color(Color.DARK_GRAY)

# Set font size
chart.set_font_size(12)

# Set font family
chart.set_font_family("-apple-system")

# Set pane height
chart.set_pane_height(pane=1, height=150)
```

### Crosshair Configuration

```python
# Set crosshair mode
chart.set_crosshair_mode(CrosshairMode.NORMAL)

# Set crosshair line style
chart.set_crosshair_line_style(horizontal=True, line_style=LineStyle.DASHED)

# Set crosshair line width
chart.set_crosshair_line_width(horizontal=True, line_width=1)

# Set crosshair line color
chart.set_crosshair_line_color(horizontal=True, line_color=Color.WHITE)

# Set crosshair line visibility
chart.set_crosshair_line_visible(horizontal=True, visible=True)

# Set crosshair line label visibility
chart.set_crosshair_line_label_visible(horizontal=True, visible=True)

# Set crosshair label background color
chart.set_crosshair_label_background_color(horizontal=True, background_color=Color.GREEN)
```

## Series

### Candlestick Series

```python
# Add a candlestick series
candlestick_series = chart.add_candlestick_series(
    pane=0,
    up_color=Color.TEAL_GREEN,
    down_color=Color.CORAL_RED,
    wick_visible=True,
    wick_color=Color.SLATE_GRAY,
    wick_up_color=Color.TEAL_GREEN,
    wick_down_color=Color.CORAL_RED,
    border_visible=True,
    border_color=Color.FOREST_GREEN,
    border_up_color=Color.TEAL_GREEN,
    border_down_color=Color.CORAL_RED,
    last_value_visible=True,
    title="BTC/USD",
    visible=True,
    price_line_visible=True,
    price_line_width=1,
    price_line_color="",  # default: last bar color
    price_line_style=LineStyle.DASHED,
    price_format_precision=2,
    price_format_min_move=0.01
)

# Set candlestick data
candlestick_data = [
    {"time": 1672531200, "open": 100, "high": 105, "low": 98, "close": 103},
    {"time": 1672617600, "open": 103, "high": 107, "low": 101, "close": 104},
    # ... more data points
]
candlestick_series.set_data(candlestick_data)

# Add OHLCV legend
ohlcv_legend = candlestick_series.add_ohlcv_legend(
    x=10, y=10,
    text_color=Color.WHITE,
    font_size=12,
    font_style="normal",
    background_color=Color.DARK_GRAY,
    border_color=Color.WHITE,
    border_width=1,
    border_radius=3,
    padding=5
)

# Customize candlestick appearance
candlestick_series.set_up_color(Color.TEAL_GREEN)
candlestick_series.set_down_color(Color.CORAL_RED)
candlestick_series.set_wick_visible(True)
candlestick_series.set_wick_color(Color.SLATE_GRAY)
candlestick_series.set_wick_up_color(Color.TEAL_GREEN)
```

### Line Series

```python
# Add a line series
line_series = chart.add_line_series(
    pane=0,
    color=Color.BRIGHT_BLUE,
    line_width=2,
    line_style=LineStyle.SOLID,
    line_type=LineType.SIMPLE,
    title="Moving Average",
    visible=True,
    price_line_visible=True,
    price_line_width=1,
    price_line_color="",  # default: line color
    price_line_style=LineStyle.DASHED
)

# Set line data
line_data = [
    {"time": 1672531200, "value": 101.5},
    {"time": 1672617600, "value": 102.3},
    # ... more data points
]
line_series.set_data(line_data)

# Customize line appearance
line_series.set_color(Color.BRIGHT_BLUE)
line_series.set_line_width(2)
line_series.set_line_style(LineStyle.SOLID)
line_series.set_line_type(LineType.SIMPLE)
line_series.set_point_markers_visible(False)
line_series.set_last_value_visible(True)
line_series.set_title("Moving Average")
line_series.set_visible(True)
line_series.set_price_line_visible(True)
line_series.set_price_line_width(1)
line_series.set_price_line_color(Color.BRIGHT_BLUE)
```

### Volume Series

```python
# Add a volume series
volume_series = chart.add_histogram_series(
    pane=1,
    color=Color.TRANS_BLUE,
    scale_margin_top=0.8,
    scale_margin_bottom=0.0,
    title="Volume",
    visible=True,
    price_line_visible=True,
    price_line_width=1,
    price_line_color="",  # default: last bar color
    price_line_style=LineStyle.DASHED
)

# Set volume data
volume_data = [
    {"time": 1672531200, "value": 1500},
    {"time": 1672617600, "value": 2100},
    # ... more data points
]
volume_series.set_data(volume_data)
```

## Chart Elements

### Background Color

The `BackgroundColor` class allows you to add colored backgrounds to specific chart areas.

### Custom Marker

The `CustomMarker` class allows you to add custom markers to the chart.

### Fibonacci

The `Fibonacci` class allows you to add Fibonacci retracement levels to the chart.

### Rectangle

The `Rectangle` class allows you to add rectangles to the chart.

### Tooltip

The `Tooltip` class allows you to add tooltips to the chart.

### Trend Line

The `TrendLine` class allows you to add trend lines to the chart.

### Vertical Line

The `VerticalLine` class allows you to add vertical lines to the chart.

### Volume Profile

The `VolumeProfile` class allows you to add volume profile visualization to the chart.

## Constants

### Line Style

```python
from vschart.constants import LineStyle

# Available line styles
LineStyle.SOLID        # 0
LineStyle.DOTTED       # 1
LineStyle.DASHED       # 2
LineStyle.LARGE_DASHED # 3
LineStyle.SPARSE_DOTTED # 4
```

### Line Type

```python
from vschart.constants import LineType

# Available line types
LineType.SIMPLE     # 0
LineType.WIDTHSTEPS # 1
LineType.CURVED     # 2
```

### Price Line Style

```python
from vschart.constants import PriceLineStyle

# Available price line styles
PriceLineStyle.SOLID        # 0
PriceLineStyle.DOTTED       # 1
PriceLineStyle.DASHED       # 2
PriceLineStyle.LARGE_DASHED # 3
PriceLineStyle.SPARSE_DOTTED # 4
```

### Crosshair Mode

```python
from vschart.constants import CrosshairMode

# Available crosshair modes
CrosshairMode.NORMAL    # 0
CrosshairMode.MAGNET    # 1
CrosshairMode.HIDDEN    # 2
CrosshairMode.MAGNETOHLC # 3
```

### Marker Position

```python
from vschart.constants import MarkerPosition

# Available marker positions
MarkerPosition.ABOVE_BAR # 'aboveBar'
MarkerPosition.BELOW_BAR # 'belowBar'
MarkerPosition.IN_BAR    # 'inBar'
```

### Marker Shape

```python
from vschart.constants import MarkerShape

# Available marker shapes
MarkerShape.CIRCLE       # 'circle'
MarkerShape.SQUARE       # 'square'
MarkerShape.ARROW_UP     # 'arrowUp'
MarkerShape.ARROW_DOWN   # 'arrowDown'
MarkerShape.STAR         # 'star'
MarkerShape.DIAMOND      # 'diamond'
MarkerShape.TRIANGLE_UP  # 'triangleUp'
MarkerShape.TRIANGLE_DOWN # 'triangleDown'
MarkerShape.RING         # 'ring'
MarkerShape.CROSS        # 'cross'
```

### Color

The `Color` class provides a wide range of predefined colors for use in charts. Some examples:

```python
from vschart.constants import Color

# Some available colors
Color.BLACK
Color.WHITE
Color.RED
Color.GREEN
Color.BLUE
Color.YELLOW
Color.TEAL_GREEN
Color.CORAL_RED
Color.DARK_GRAY
# ... and many more
```

### Size

```python
from vschart.constants import Size

# Available sizes
Size.XS    # 4
Size.S     # 8
Size.M     # 12
Size.L     # 16
Size.XL    # 20
Size.XXL   # 24
Size.XXXL  # 28
Size.XXXXL # 32
```
