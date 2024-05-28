---
title: PolylineLayer
sidebar_label: PolylineLayer
---

A layer to display [`PolylineMarker`](#polylinemarker-properties)s.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## Examples

See [`Map`](/docs/controls/map) Control.

## `PolylineLayer` Properties

### `polylines`

A list of [`PolylineMarker`](#polylinemarker-properties)s to be displayed.

## `PolylineMarker` Properties

A marker for the [`PolylineLayer`](#polylinelayer-properties).

### `border_color`

The border color of this polyline.

### `border_stroke_width`

The border stroke width of this polyline. Defaults to `0.0` - disabled.

### `color`

The color of this polyline marker.

### `colors_stop`

A list of stops for gradient colors along the polyline.

### `coordinates`

The list of coordinates (latitude and longitude) defining the polyline. Value is of type [`MapLatitudeLongitude`](/docs/reference/types/maplatitudelongitude).

### `dotted`

Whether the polyline should be dotted. Defaults to `False`.

### `gradient_colors`

A list of colors in case a gradient should get used.

### `stroke_cap`

The stroke cap style of the polyline. Value is of type [`StrokeCap`](/docs/reference/types/strokecap).

### `stroke_join`

The stroke join style of the polyline. Value is of type [`StrokeJoin`](/docs/reference/types/strokejoin).

### `stroke_width`

The stroke width of this polyline.

### `use_stroke_width_in_meter`

A boolean value to indicate if the stroke width is in meters or pixels. Defaults to `False` - pixels.
