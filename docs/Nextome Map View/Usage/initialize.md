# Initialize the NextomeMapView Component

!!!note
    If you are working with Android first add this on your activity.xml file

        <com.nextome.nextomemapview.NextomeMapView
            android:id="@+id/mapview"
            android:forceHasOverlappingRendering="true"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

=== "Android"
    ```kotlin

    // Declare the NextomeMapViewHandler
    // You can declare it at start of your MainActivity scope
    val mapview: NextomeMapViewHandler = NextomeMapViewHandler()

    ...

    // Initialize the mapview handler
    mapview.initialize(
        fragmentManager = supportFragmentManager,
        viewId = R.id.mapview, 
        this
    )

    // Create the first tile
    var tile: NMTile = NMTile()
    tile.show = true
    tile.name = "Tile1"
    tile.id = "1234"
    tile.source = "PATH TO THE .ZIP"

    // Set the resource to show on map
    mapview.setResources(tiles = listOf(tile), zoom = 1, width = 2000, height = 1000)
    ```
=== "iOS"
    ```swift

        // Import the module
        import NextomeMapView

        // Call initialize() inside application(_, didFinishiLaunchingWithOptions) of your AppDelegate 
        NextomeMapViewHandler.instance.initialize()

        // Call method below in viewDidLoad of your UIViewController
        var vc = NextomeMapViewHandler.instance.initializeFlutterViewController()
        self.present(vc, animated: true)
        
        // Create the tile to show
        var tile: NMTile = NMTile(name: "Tile1", id: "1234", source: "PATH TO THE .ZIP")

        // Set the resource to show on map
        NextomeMapViewHandler.instance.setResources(tiles: [tile], zoom: 1, width: 2000, height: 1000)
    ```

!!!note
    The Tile declaration and the call of `setResources` method is mandatory to load and show elements on MapView. Without a background resource like a Tile, the map can't be fully initialized.
    <br>
    <br>You can define multiple tiles map and pass them into a list int setResources method. 
    <br>
    <br>We discourage setResources after the map has built becouse the plugin need to elaborate the zip file, so if you can, pass all you need tiles once at when you start to use the MapView. 
    <br>
    <br>To avoid strange behaviour, the different tiles need to have same width and height.
