# Tiles

The Tile is the background component used to show the map. 
<br>The tile declaration is mandatory to get MapView initialized. 
<br>Can be possibile managed the tile behaviour like visibility but can't be possibile to add a new tile or remove an existing tile. The only way is re-assign the whole resources like the initialization phase.

The map graphics will be imported as `zip` file where the tiles images and folder need to be structured usind `Deep Zoom` criteria and need to match the pattern `/{z}/{x}/{y}` where
<br>- {x}: x axis coordinate
<br>- {y}: y axis coordinate
<br>- {z}: zoom level
<!--- more https://docs.fleaflet.dev/layers/tile-layer -->

## Class description

The `NMTile` class is described below

<!--- === "Dart"
    | Property             | Description                          | Default |
    | :--------------------| :----------------------------------- | :-------
    | `name: String`   | The name assigned the tile   | null |
    | `id: String`   | The identification assigned to the tile   | null |
    | `source: String`  | The source path of the zip  | null |
    | `zindex: Int`   | The sorting rendering index | 0 |
    | `visible: Bool`| Set if the tile is visible or not | true |  
-->
=== "Android"
    | Property             | Description                          | Default |
    | :--------------------| :----------------------------------- | :-------
    | `name: String?`   | The name assigned the to tile   | null |
    | `id: String?`   | The identification assigned to the tile   | null |
    | `source: String?`  | The source path of the zip  | null |
    | `zindex: Int`   | The sorting rendering index | 0 |
    | `visible: Bool`| Set if the tile is visible or not | true | 
=== "iOS"
    | Property             | Description                          | Default |
    | :--------------------| :----------------------------------- | :-------
    | `name: String?`   | The name assigned to the tile   | nil |
    | `id: String?`   | The identification assigned to the tile   | nil |
    | `source: String?`  | The source path of the zip  | nil |
    | `zindex: Int`   | The sorting rendering index | 0 |
    | `visible: Bool`| Set if the tile is visible or not | true | 


## Create and set Tile

<!--- === "Dart"
    ```Dart 
    NMTile tile = NMTile(
        name: 'Tile Name',
        id: 'Tile Id',
        zindex: 0,
        source: 'assets/tile_package.zip',
    );
    ``` --> 
=== "Android"
    ```kotlin
    var tile: NMTile = NMTile()
    tile.show = true
    tile.name = "Tile1"
    tile.id = "2314"
    tile.source = "PATH TO ZIP FILE"

    mapview.setResources(tiles = listOf(tile), zoom = 1, width = 2000, height = 1000)
    ```
=== "iOS"
    ```swift
    var tile: NMTile = NMTile(
        name: "Tile1", 
        id: "1234", 
        source:  "PATH TO ZIP FILE")

    NextomeMapViewHandler.instance.setResources(tiles: [tile], zoom: 1, width: 2000, height: 1000)
    ```

## Show/hide an existing Tile

<!--- === "Dart"
    ```Dart 
    NOT IMPLEMENTED IN ANDROID AND IOS
    ```
-->
=== "Android"
    ```kotlin
    mapview.setTileVisibility(tileId: String, show: Boolean)
    mapview.apply()
    ```
=== "iOS"
    ```swift
    NextomeMapViewHandler.instance.setTileVisibility(tileId: String, show: Bool)
    NextomeMapViewHandler.instance.apply()
    ```
