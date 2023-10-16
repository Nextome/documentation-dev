# Events

The NextomeMapView provide some events to manage the MapView and User behaviours.

## ON READY EVENT
To check if resources are loaded, you can subscribe to OnMapReady event. This is helpful to know when all tile resources 
are loaded. You can start to populate the map with markers after this event.

=== "Android"
    ```kotlin
    // Suppose mapview has been declared in some part of your code
    val mapview: NextomeMapViewHandler = NextomeMapViewHandler()
    ...
    mapview.setOnMapReady {
        println("EVENT ON MAP READY")
    }
    ...
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.setOnMapReady(callback: mapReady)
    ...
    func mapReady() -> Void {
        print("CALLBACK MAP IS READY")
    }
    ```

## ON MAP TAP
OnMapTap event return the tapped point on the map. X and Y can be intendeed like position in pixel of the tap (based on width and height) or can be intedeed as latitude and longitude in accordance to your map usage.

=== "Android"
    ```kotlin
    // Suppose mapview has been declared in some part of your code
    val mapview: NextomeMapViewHandler = NextomeMapViewHandler()
    ...
    mapview.setOnMapTap {  x, y ->
        println("EVENT MAP TAP $x $y")
    }
    ...
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.setOnMapTap(callback: mapTap)
    ...
    func mapTap(x: Double, y: Double) -> Void {
        print("CALLBACK MAP TAP \(x) \(y)")
    }
    ```

## ON MAP LONG PRESS
OnMapLongPress event return the long pressed point on the map. X and Y can be intendeed like position in pixel of the tap (based on width and height) or can be intedeed as latitude and longitude in accordance to your map usage.

=== "Android"
    ```kotlin
    // Suppose mapview has been declared in some part of your code
    val mapview: NextomeMapViewHandler = NextomeMapViewHandler()
    ...
    mapview.setOnMapLongPress {  x, y ->
        println("EVENT MAP LONG PRESS $x $y")
    }
    ...
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.setOnMapLongPress(callback: mapLongPress)
    ...
    func mapLongPress(x: Double, y: Double) -> Void {
        print("CALLBACK MAP LONG PRESS \(x) \(y)")
        populate()
    }
    ```

## ON MARKER TAP
OnMarkerTap event return the marker tapped

=== "Android"
    ```kotlin
    // Suppose mapview has been declared in some part of your code
    val mapview: NextomeMapViewHandler = NextomeMapViewHandler()
    ...
    mapview.setOnMarkerTap { marker ->
        println("EVENT MARKER TAP ${marker.id}")
    }
    ...
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.setOnMarkerTap(callback: markerTap)
    ...
    func markerTap(marker: NMMarker) -> Void {
        print("CALLBACK MARKER TAP ON \(marker.id ?? "ID IS NULL")")
    }
    ```