# Layer

The layer component hold a group of markers. It can be used to organize the informations that you wanto to show on the map.
For example, suppose to manage an emergency evacuation plan, you can show on map emergency path and exits and fire hoses. 
You can create two layers, "Paths" and "Fire Hoses", and distribuite the items between this two layers. If you want to see only
emergency path, just hide "Fire Hoses" layer.

<!---
## Class description

The `NMLayer` class is described below

 === "Dart"=== "Dart"
| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `name: String`   | The name assigned the layer   | null |
| `id: String`   | The identification assigned to the layer   | null |
| `markers: List<NMBaseItem>`  | A list of markers | null |
| `zindex: Int`   | The sorting rendering index   | 0 |
| `visible: Bool`| Set if the layer is visible or not | true | 

=== "Android"
| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `name: String`   | The name assigned the layer   | null |
| `id: String`   | The identification assigned to the layer   | null |
| `markers: List<NMBaseItem>`  | A list of markers | null |
| `zindex: Int`   | The sorting rendering index   | 0 |
| `visible: Bool`| Set if the layer is visible or not | true | 
=== "iOS"
| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `name: String`   | The name assigned the layer   | null |
| `id: String`   | The identification assigned to the layer   | null |
| `markers: List<NMBaseItem>`  | A list of markers | null |
| `zindex: Int`   | The sorting rendering index   | 0 |
| `visible: Bool`| Set if the layer is visible or not | true | 
-->

## Create a Layer

Create a new layer to hold a group of markers.  

=== "Android"
    ```kotlin
    mapview.addLayer(layerId: "LayerA")
    mapview.apply()
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.addLayer(layerId: "LayerA")
    NextomeMapViewHandler.instance.apply()
    ```

## Set Layer visibility

Set the visibility of a layer

=== "Android"
    ```kotlin
    mapview.setLayerVisibility(layerId: "LayerA", show: false)
    mapview.apply()
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.setLayerVisibility(layerId: "LayerA", show: false)
    NextomeMapViewHandler.instance.apply()
    ```

## Clear a Layer

Delete all markers presents into the layer

=== "Android"
    ```kotlin
    mapview.clearLayer(layerId: "LayerA")
    mapview.apply()
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.clearLayer(layerId: "LayerA")
    NextomeMapViewHandler.instance.apply()
    ```

## Example usage

!!!note 
    The NMLayer is actually managed by the controller, therefore the developer doesn't need to write any code about that.
    See the NMMapController class to see how manage layers.