# Nextome MapView class

The controller is the interface that the developer see to manage the Nextome Map View.
With the controller tha developer can add, modify and remove layers and markers. Can also manage the visibility of the tiles,
can navigate programatically on the map and can hande the incoming events like taps.

## Declaration

!!!note
    The whole declaration process is described into [Initialize](./Usage/initialize.md) section of NextomeMapView documentation.

=== "Android"
    ```kotlin
    val mapview: NextomeMapViewHandler = NextomeMapViewHandler()
    ```
=== "iOS"
    ```swift
    // Called in your AppDelegate didFinishLaunchingWithOptions
    NextomeMapViewHandler.instance.initialize()

    // Called in your UIViewController viewDidLoad
    var vc = NextomeMapViewHandler.instance.initializeFlutterViewController()
    self.present(vc, animated: true)
    ```


## Initialize

=== "Android"
    ```kotlin
    fun initialize(
        fragmentManager: FragmentManager,
        @IdRes viewId :Int,
        context: Context,
    ): Fragment
    ```
=== "iOS"
    ```swift
    // Called in your AppDelegate didFinishLaunchingWithOptions
    public func initialize()
    // Called in your UIViewController viewDidLoad
    public func initializeFlutterViewController() -> FlutterViewController
    ```

## Important! the APPLY method

A very important thing to know about the controller is that to apply a change to the internal state of the map, is mandatory to call the `apply()` method. For example if you need to add a marker, or turn-off a layer you must call `apply()` method to see this changes. Also you can do collect actions like add, modifying, removing, turn-off/on and so on, but at the end you must to call `apply()` method to see all this change applied. If you don't call the `apply()` method after your changes, internally the changes are applied but nothing appen to on the screen.

=== "Android"
    ```kotlin
    fun apply()
    ```
=== "iOS"
    ```swift
    public func apply()
    ```

## Set resources

Set the resources to initialize the map. In the specific:

- A list of tiles
- The zoom level intendeed as the folder depth of provided zip
- The width and the height of the map related to the size in pixel of the map
- A flag useMockView that allow to use the map without pass any tile, just for test
- A flag isLatLng that specify if the coordinate system used will be pixel or latitude-longitude

=== "Android"
    ```kotlin
    fun setResources(tiles: List<NMTile>, zoom: Int, width: Int, height: Int, useMockView: Boolean = false, isLatLng: Boolean = false)
    ```
=== "iOS"
    ```swift
    public func setResources(tiles: [NMTile], zoom: Int, width: Int, height: Int, useMockView: Bool = false, isLatLng: Bool = false) -> Void
    ```

## Manage Tiles

### Show-hide Tiles

=== "Android"
    ```kotlin
    fun setTileVisibility(tileId: String, show: Boolean)
    ```
=== "iOS"
    ```swift
    public func setTileVisibility(tileId: String, show: Bool)
    ```

## Manage Layers

### Add Layer

To add a new laye:

=== "Android"
    ```kotlin
    fun addLayer(layerId: String)
    ```
=== "iOS"
    ```swift
    public func addLayer(layerId: String)
    ```

### Show-hide Layer

=== "Android"
    ```kotlin
    fun setLayerVisibility(layerId: String, show: Boolean)
    ```
=== "iOS"
    ```swift
    public func setLayerVisibility(layerId: String, show: Bool)
    ```

### Clear Layer

Remove all marker from a specific layer

=== "Android"
    ```kotlin
    fun clearLayer(layerId: String)
    ```
=== "iOS"
    ```swift
    public func clearLayer(layerId: String)
    ```

To remove all marker from all layers

=== "Android"
    ```kotlin
    fun clearAll()
    ```
=== "iOS"
    ```swift
    public func clearAll()
    ```

## Manage Markers (Marker, Path, Shape)

### Add Marker
To add a marker to a layer

=== "Android"
    ```kotlin
    fun addMarker(layerId: String, marker: NMMarker)
    fun addPath(layerId: String, path: NMPath)
    fun addShape(layerId: String, shape: NMShape)
    ```
=== "iOS"
    ```swift
    public func addMarker(layerId: String, marker: NMMarker)
    public func addPath(layerId: String, path: NMPath)
    public func addShape(layerId: String, shape: NMShape)
    ```

### Update Marker

=== "Android"
    ```kotlin
    fun updateMarker(layerId: String, marker: NMMarker)
    fun updatePath(layerId: String, path: NMPath)
    fun updateShape(layerId: String, shape: NMShape)
    ```
=== "iOS"
    ```swift
    public func updateMarker(layerId: String, marker: NMMarker)
    public func updatePath(layerId: String, path: NMPath)
    public func updateShape(layerId: String, shape: NMShape)
    ```

### Get Marker

To get a marker, call:
=== "Dart"
    ```Dart 
    NMMarker? getMarker(String layerId, String markerId);
    NMWidgetMarker? getWidgetMarker(String layerId, String markerId);
    NMPath? getPath(String layerId, String pathId);
    NMShape? getShape(String  layerId, String shapeId);
    ```
=== "Android"
    ```kotlin
    TODO
    ```
=== "iOS"
    ```swift
    TODO
    ```


## Map behaviour control

### Go to point

Move on a specific point. Note that the point must match the coordinates system choose during initialization.
You can pass pixel coordinates or lat-long coordinates.

=== "Android"
    ```kotlin
    fun goToPosition(x: Double, y: Double)
    ```
=== "iOS"
    ```swift
    public func goToPosition(x: Double, y: Double)
    ```

### Set Zoom

Set the zoom

=== "Android"
    ```kotlin
    fun setZoom(zoom: Double)
    ```
=== "iOS"
    ```swift
    public func setZoom(zoom: Double)
    ```

<!--- ### Set

Show a default map scale-bar
=== "Dart"
    ```Dart 
    void showScaleBar(bool show);  // NOT IMPLEMENTED YET
    ```
=== "Android"
    ```kotlin
    TODO
    ```
=== "iOS"
    ```swift
    TODO
    ```
    void startCompass()
-->

### Enable/Disable the map orientation using device compass

=== "Android"
    ```kotlin
    fun useCompass(use: Boolean)
    ```
=== "iOS"
    ```swift
    public func useCompass(use: Bool)
    ```


## Manage Events

### Map Ready

Called when all tiles are loaded and showed

=== "Android"
    ```kotlin
    fun setOnMapReady( callback: () -> Unit){ this.onMapReady = callback }
    ```
=== "iOS"
    ```swift
    private var onMapReady: (() -> Void)? = nil
    ```


### Map Tap Events

When an empty point on the map is tapped or is pressed for a long time

=== "Android"
    ```kotlin
    fun setOnMapTap(callback: (Double, Double) -> Unit){ this.onMapTap = callback }
    fun setOnMapLongPress(callback: (Double, Double) -> Unit){ this.onMapLongPress = callback }
    ```
=== "iOS"
    ```swift
    private var onMapTap: ((Double, Double) -> Void)? = nil
    private var onMapLongPress: ((Double, Double) -> Void)? = nil
    ```

### Marker Events

Called when a marker (just NMMarker object, not NMPath and NMShape) is tapped

=== "Android"
    ```kotlin
    fun setOnMarkerTap(callback: (NMMarker) -> Unit){ this.onMarkerTap = callback }
    ```
=== "iOS"
    ```swift
    private var onMarkerTap: ((NMMarker) -> Void)? = nil
    ```