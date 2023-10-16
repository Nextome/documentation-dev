# Markers

A marker is the graphics componend that show an information in a exact point of the map.
NextomeMapView manage three different type of markers:

- Marker, the generic marker represented by an image
- Path, a list of points connected by a customizable line
- Shape, a list of points that represented a polygon
<!--- - NMWidgetMarker (FLUTTER ONLY)-->

## Marker

The `NMMarker`. Is a simple marker represented by an image. Support `jpeg`, `png` and `gif` and it can be loaded from an url, from the Flutter asset folder and from a custom folder on the device.

### Marker class description

| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `id: String`   | The identification assigned to the marker   | null |
| `objType: String`  | Hold the type of marker if the developer want to take trace about that  | null |
| `objMeta: String`  | Hold some custom information if the developer want to take trace about that | null |
| `visible: Bool`| Set if the markr is visible or not | null | 
| `x: Double`   | Determinate the x position on the map | 0 |
| `y: Double`   | Determinate the y position on the map | 0 |
| `anchor: NMAnchorPoint`   | Determinate the center of the marker based on x,y position | CENTER |
| `zindex: Int`   | The sorting rendering index  | 0 |
| `width: Double`   | Determinate the width of the marker | 0 |
| `height: Double`   | Determinate the height of the marker| 0 |
| `rotate: Bool`   | Determinate if the marker rotate with the map or if it remains screen-vertical oriented | false |
| `scale: Bool`   | Determinate if the marker scale with the map or if it remains with a fixed screen-size| false |
| `source: String`   | Determinate the path of the image that can be JPEG, PNG or GIF | null |
| `sourceType: NMMSourceType`   | Determinate the source type of the image, it can be from an url, from Flutter folder asset, or a custom platform path | INTERNET |
| `onTap: Function`   | Callback event fired when the widget is tapped | null |

### Add a Marker

Add a Marker to an existing layer called "LayerA"

<!--- === "Dart"
    ```Dart 
    NMMarker marker = NMMarker(
        id: 'MARKER ID',
        x: 1000,
        y: 1200,
        width: 50,
        height: 50,
        source: 'https://sampleimage.jpg',
        sourceType: NMMSourceType.INTERNET,
    );
    ```
-->
=== "Android"
    ```kotlin
    var marker: NMMarker = NMMarker()
    marker.id = "1234"
    marker.x = 1000.0
    marker.y = 1000.0
    marker.height = 30.0
    marker.width = 30.0
    marker.source = "PATH TO IMAGE"
    marker.sourceType = NMSourceType.FILESYSTEM

    mapview.addMarker("layerA", marker)
    mapview.apply()
    ```
=== "iOS"
    ```swift
    var marker: NMMarker = NMMarker()
    marker.id = "1234"
    marker.x = 1000.0
    marker.y = 1000.0
    marker.height = 30.0
    marker.width = 30.0
    marker.source = "PATH TO IMAGE"
    marker.sourceType = NMSourceType.FILESYSTEM

    NextomeMapViewHandler.instance.addMarker(layerId: "layerA", marker: marker)
    NextomeMapViewHandler.instance.apply()
    ```

## Path

The `NMPath`. Is marker that show a path on a map.

### Path Class description

| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `id: String`   | The identification assigned to the marker   | null |
| `objType: String`  | Hold the type of marker if the developer want to take trace about that  | null |
| `objMeta: String`  | Hold some custom information if the developer want to take trace about that | null |
| `visible: Bool`| Set if the markr is visible or not | null | 
| `zindex: Int`   | The sorting rendering index  | 0 |
| `points: List<NMPoint>`   | A list of path points | [ ] |
| `width: Double`   | Determinate the stroke width | 1 |
| `color: Color`   | The color of the path | null |
| `style: NMLineStyle`   | The style of the path, NORMAL or DOT | NORMAL |

### Add a Path

Add a Path to an existing layer called "LayerA"

<!--- === "Dart"
    ```Dart 
    NMPath path = NMPath(
        id: 'PATH ID',
        color: Colors.pink,
        width: 4,
        points: [NMPoint(x:1000, y:1000), NMPoint(x:1000, y:5000)]
    );
    ```
-->
=== "Android"
    ```kotlin
    var path: NMPath = NMPath()
    path.color = NMColor(255, 100, 100)
    path.width = 2.0;
    path.style = NMLineStyle.DOT
    path.id = "path_id"
    path.points = listOf(NMPoint(1000.0, 1000.0), NMPoint(3000.0, 3000.0))

    mapview.addPath("layerA", marker)
    mapview.apply()
    ```
=== "iOS"
    ```swift
    var path: NMPath = NMPath()
    path.color = NMColor(r: 255, g: 100, b: 100)
    path.width = 2.0;
    path.style = NMLineStyle.DOT
    path.id = "path_id"
    path.points = [NMPoint(x: 1000.0, y: 1000.0), NMPoint(x: 3000.0, y: 3000.0)]

    NextomeMapViewHandler.instance.addPath(layerId: "layerA", path:path)
    NextomeMapViewHandler.instance.apply()
    ```

## Shape

The `NMShape`. Is marker that show a shape on a map.

### Shape Class description

| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `id: String`   | The identification assigned to the marker   | null |
| `objType: String`  | Hold the type of marker if the developer want to take trace about that  | null |
| `objMeta: String`  | Hold some custom information if the developer want to take trace about that | null |
| `visible: Bool`| Set if the markr is visible or not | null | 
| `zindex: Int`   | The sorting rendering index  | 0 |
| `x: Double`   | Determinate the center x position on the map | 0 |
| `y: Double`   | Determinate the center y position on the map | 0 |
| `points: List<NMPoint>`   | A list of shape points | null |
| `radius: Double`   | The radious of the circle shape | null |
| `relative: Bool`   | Determinate if the points positions are relative to the center or are in absolute values | false |
| `borderWidth: Double`   | Determinate the stroke width | 1 |
| `borderColor: Color`   | The color of the path | null |
| `borderStyle: NMLineStyle`   | The style of the path, NORMAL or DOT | false |
| `fillColor: Color`   | Determinate the fill color | null |
| `label: String`   | Determinate a text can be shown at the center of the shape | null |
| `rotateLabel: Bool`   | Determinate if the label have to rotate with the map or it have to be vertical aligned with the screen | false |
| `labelStyle: String`   | The label text style provided as JSON *(NOT IMPLEMENTED YET)* | null |
| `holePoints: List<NMPoint>`   | A list of points represented an hole into the shape | null |

### Add a Shape

Add a Shape to an existing layer called "LayerA"

<!--- === "Dart"
    ```Dart 
    NMShape shape = NMShape(
        id: 'SHAPE ID',
        points: [NMPoint(x: 2400, y:1245), NMPoint(x: 3400, y:1545), NMPoint(x: 1200, y:5320)],
        x: 2500,
        y: 3500,
        fillColor: Colors.deepPurple,
        borderColor: Colors.amber,
        borderWidth: 6,
        borderStyle: NMLineStyle.NORMAL
    );
    ```
-->
=== "Android"
    ```kotlin
    var shape: NMShape = NMShape()
    shape.id = "ShapeId"
    shape.borderWidth = 3.0
    shape.borderColor = NMColor(100, 255, 255)
    shape.fillColor = NMColor(100, 100, 255, 0.23)
    shape.relative = false
    shape.points = listOf(NMPoint(1300.0, 1500.0), NMPoint(3500.0, 3500.0), NMPoint(1300.0,3500.0))

    mapview.addShape("layerA", marker)
    mapview.apply()
    ```
=== "iOS"
    ```swift
    var shape: NMShape = NMShape()
    shape.id = "ShapeId"
    shape.borderWidth = 3.0
    shape.borderColor = NMColor(r: 100, g: 255, b: 255)
    shape.fillColor = NMColor(r: 100, g: 100, b: 255, a: 0.23)
    shape.relative = false
    shape.points = [NMPoint(x: 1300.0, y: 1500.0), NMPoint(x: 3500.0, y: 3500.0), NMPoint(x: 1300.0, y: 3500.0)]

    NextomeMapViewHandler.instance.addShape(layerId: "layerA", shape: shape)
    NextomeMapViewHandler.instance.apply()
    ```