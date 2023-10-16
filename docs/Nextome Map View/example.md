# Nextome Map View Example

Below a simple commented example to show how implement and use the Nextome Map View

<!--- 
=== "Dart"
    ```Dart 
    class Example extends StatefulWidget {
        @override
        _ExampleState createState() => _ExampleState();
    }

    class _ExampleState extends State<Example> {

        NMMapView? mapview;
        NMMapController? controller;

        @override
        void initState() {
            super.initState();

            mapview = buildMapViewWidget(context);
        }

        // Build the main map view widget
        NMMapView? buildMapViewWidget(BuildContext context){

            // Original png image height and width
            final double mapHeight = 7500;
            final double mapWidth = 12000;

            // Declare the tile
            NMTile tile = NMTile(
                name: 'FIRST FLOOR',
                id: '1',
                zindex: 0,
                source: 'PATH_TO_THE_ZIP.zip',
            );

            // Build a list of tiles
            List<NMTile> tiles = [tile];

            // Initialize the controller
            controller = NMMapController();

            // Subscribe to controller events
            controller!.onMapReady = (){
                // Map is ready, can be popolated now
                populate();
            };

            controller!.onMarkerTap = (NMMarker marker){
                // Do nothing, just print
                print("PRESSED: " + marker.id!);
            };

            controller!.onMarkerLongPress = (NMMarker marker){
                // Do nothing, just print
                print("LONG PRESSED: " + marker.id!);
            };

            controller!.onMapTap = (p0, p1) {
                // Do nothing, just print
                print('PO P1 ' + p0.toString() + ' ' + p1.toString());
            };

            controller!.onMapLongPress = (p0, p1) {
                // Do nothing, just print
                print('LON PRESS PO P1 ' + p0.toString() + ' ' + p1.toString());
            };

            // Build the main Map widget
            NMMapView mapview = NMMapView(
                mapWidth: mapWidth,
                mapHeight: mapHeight,
                tiles: tiles,
                controller: controller,
                loadMockedMap: false,
            );
            
            return mapview;
        }

        // Generate the markers in temporized mode to show how to use correctly the controller
        void populate() async {

            // Wait for 5 seconds and generate 3 markers
            await Future.delayed(Duration(seconds: 5));

            NMMarker marker1 = NMMarker(
                id: 'marker_1',
                x: 1000,
                y: 7000,
                width: 50,
                height: 50,
                source: 'PATH_TO_AN_ASSET_IMAGE',
                sourceType: NMMSourceType.ASSET,
            );

            // Rotate and scale with the map
            NMMarker marker2 = NMMarker(
                id: 'GIOACCHINO',
                x: 4000,
                y: 2000,
                width: 1000,
                height: 1000,
                source: 'PATH_TO_AN_ASSET_IMAGE',
                rotate: true,
                scale: true,
                sourceType: NMMSourceType.ASSET,
            );

            // Rotate with the map
            NMMarker marker3 = NMMarker(
                id: 'GILIBERTO',
                x: 6000,
                y: 6000,
                width: 75,
                height: 75,
                rotate: true,
                source: 'PATH_TO_AN_ASSET_IMAGE,
                sourceType: NMMSourceType.ASSET,
            );

            // Add a new layer called layerA, assign the markers to this layer and call the apply() method to see the map updated in UI
            controller!.addLayer('layerA');
            controller!.addMarkers('layerA', [marker1, marker2, marker3]);
            controller!.apply();

            // Wait for 2 seconds and generate a WidgetMarker
            await Future.delayed(Duration(seconds: 2));

            NMWidgetMarker widgetMarker = NMWidgetMarker(
                x: 3500,
                y: 3000,
                width: 50,
                height: 50,
                builder: (context) {
                return Container(
                    height: 350,
                    width: 350,
                    decoration: BoxDecoration(
                    shape: BoxShape.circle,
                    color: Colors.red,
                    ),
                    child: Icon(Icons.bluetooth_audio_sharp, color: Colors.white,),
                );
                },
            );

            // Add the widget marker to existing layerA and call the apply()
            controller!.addWidgetMarkers('layerA', [widgetMarker]);
            controller!.apply();   

            // Wait for 2 seconds and generate 2 paths
            await Future.delayed(Duration(seconds: 2));

            NMPath path1 = NMPath(
                id: 'Path1',
                color: Colors.pink,
                width: 4,
                points: [NMPoint(x:1000, y:1000), NMPoint(x:1000, y:5000)]
            );

            // Dotted path
            NMPath path2 = NMPath(
                id: 'Path2',
                color: Colors.green,
                width: 4,
                style: NMLineStyle.DOT,
                points: [NMPoint(x:4000, y:2000), NMPoint(x:5000, y:5000), NMPoint(x:7000, y:1000)]
            );

            // Add a new layer called layerB that hold the paths, add them to layerB and call apply()
            controller!.addLayer('layerB');
            controller!.addPaths('layerB', [path1, path2]);
            controller!.apply();

            // Wait for 2 seconds and generate a circle shape and a polygon shape
            await Future.delayed(Duration(seconds: 2));

            NMShape shape1 = NMShape(
                id: 'Shape1',
                radius: 1000, // <- mark this shape as a circle
                x: 3000,
                y: 3500,
                fillColor: Colors.red.withOpacity(0.4),
                borderColor: Colors.blue,
                borderWidth: 3,
                borderStyle: NMLineStyle.DOT
            );

            NMShape shape2 = NMShape(
                id: 'Shape2',
                points: [NMPoint(x: 2400, y:1245), NMPoint(x: 3400, y:1545), NMPoint(x: 1200, y:5320)],
                x: 2500,
                y: 3500,
                fillColor: Colors.deepPurple,
                borderColor: Colors.amber,
                borderWidth: 6,
                borderStyle: NMLineStyle.NORMAL
            );

            // Add a new layer called layerC that hold the shapes, add them to layerC and call apply()
            controller!.addLayer('layerC');
            controller!.addShapes('layerC', [shape1, shape2]);
            controller!.apply();

            // Wait 2 second and start to modify path and shape
            await Future.delayed(Duration(seconds: 2));
            increaseCircle();
            increasePath();
        }

        // Increase the circle radius
        void increaseCircle() async {

            Future.doWhile(() async {
            
                await Future.delayed(Duration(milliseconds: 100));

                // Get the circle shape from its layer
                NMShape? circle = controller!.getShape('layerC', 'Shape1');
                if(circle != null){

                    // Increase the radious
                    circle.radius = circle.radius! + 25;
                    if(circle.radius! > 1500){
                    circle.radius = 100;
                    }

                    // Update the shape
                    controller!.updateShape('layerC', circle);
                }
                
                // Apply to see the changes on the UI
                controller!.apply();
                return true;
            });
        }

        // Add new points on path
        void increasePath() async {

            Future.doWhile(() async {
            
                await Future.delayed(Duration(seconds: 2));

                // Get the path from its layer
                NMPath? path = controller!.getPath('layerB', 'Path1');
                if(path != null){
                    
                    // Add a point
                    path.points.add(NMPoint(x: Random().nextInt(10000).toDouble(), y: Random().nextInt(5000).toDouble()));

                    // Update the path
                    controller!.updatePath('layerB', path);
                }
                
                // Apply to see the changes on the UI
                controller!.apply();
                return true;
            });
        }

        @override
        Widget build(BuildContext context) {

            mapview = buildMapViewWidget(context);

            return MaterialApp(
            title: 'Nextome Map View 2',
            theme: ThemeData(
                primarySwatch: Colors.blue,
            ),
            home: Scaffold(
                body: Builder(builder: (context) {
                    return Stack(
                        children: [
                            // Map Widget
                            Positioned(
                                child: mapview ?? Container(
                                height: 100,
                                width: 100,
                                color: Colors.pink,
                                    ),
                            ),
                            // Buttons row to move on a random position and start and stop the map auto-orientation mode with the compass
                            Positioned(
                                bottom: 10,
                                left: 10,
                                right: 0,
                                height: 50,
                                child: Row(
                                    children: [
                                
                                        // Move on random position
                                        ElevatedButton(
                                            onPressed: (){ 
                                                controller!.goToXY(
                                                    Random().nextInt(12000).toDouble(), 
                                                    Random().nextInt(7000).toDouble(), 
                                                true);
                                            }, 
                                            child: Text('Move Random'),
                                        ),
                                        SizedBox(width: 24,),

                                        // Start the compass
                                        ElevatedButton(
                                            onPressed: (){ 
                                                controller!.startCompass();
                                            }, 
                                            child: Text('Start compass'),
                                        ),
                                        SizedBox(width: 24,),

                                        // Stop the compass
                                        ElevatedButton(
                                            onPressed: (){ 
                                                controller!.stopCompass();
                                            }, 
                                            child: Text('Stop compass'),
                                        ),
                                    ],
                                ),
                            ),
                        ],
                    );
                },
            ),),);
        }
    }
    ```
-->

In this example we suppose to have tiles of 12000 pixel width and 6500 pixel height.

=== "Android"
    ```kotlin
    class MainActivity : AppCompatActivity() {
        
        val mapview: NextomeMapViewHandler = NextomeMapViewHandler()

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            mapview.initialize(
                fragmentManager = supportFragmentManager,
                viewId = R.id.mapview,
                this
            )

            var tile: NMTile = NMTile()
            tile.show = true
            tile.name = "Tile1"
            tile.id = "1234"
            tile.source = "PATH TO ZIP FILE"

            mapview.setResources(tiles = listOf(tile), zoom = 1, width = 12000, height = 6500)

            lifecycleScope.launch {
                repeatOnLifecycle(Lifecycle.State.STARTED) {
                    mapview.observeEvents().collect { event ->
                        println(".EVENT -> $event")
                    }
                }
            }

            mapview.setOnMapReady {
                println("EVENT ON MAP READY")
                populate()
            }

            mapview.setOnMapTap {  x, y ->
                println("EVENT MAP TAP $x $y")
            }

            mapview.setOnMapLongPress {  x, y ->
                println("EVENT MAP LONG PRESS $x $y")
            }

            mapview.setOnMarkerTap { marker ->
                println("EVENT MARKER TAP ${marker.id}")
            }
        }

        fun populate(){

            // Marker placed at position 1000, 1000
            var marker: NMMarker = NMMarker()
            marker.id = "image_id"
            marker.x = 1000.0
            marker.y = 1000.0
            marker.height = 30.0
            marker.width = 30.0
            marker.source = "PATH TO IMAGE FILE"
            marker.sourceType = NMSourceType.FILESYSTEM

            // Path described by 2 points from 1000,1000 to 3000,3000
            var path: NMPath = NMPath()
            path.color = NMColor(255, 100, 100)
            path.width = 2.0;
            path.style = NMLineStyle.DOT
            path.id = "path_id"
            path.points = listOf(NMPoint(1000.0, 1000.0), NMPoint(3000.0, 3000.0))

            // Shape like triangle described by 3 points 
            var shape: NMShape = NMShape()
            shape.id = "shape_1_id"
            shape.borderWidth = 3.0
            shape.borderColor = NMColor(100, 255, 255)
            shape.fillColor = NMColor(100, 100, 255, 0.23)
            shape.relative = false
            shape.points = listOf(NMPoint(1300.0, 1500.0), NMPoint(3500.0, 3500.0), NMPoint(1300.0,3500.0))

            // Shape described as circle setted by radius
            var shapeCircle: NMShape = NMShape()
            shapeCircle.id = "shape_2_id"
            shapeCircle.borderWidth = 3.0
            shapeCircle.borderColor = NMColor(100, 255, 100)
            shapeCircle.fillColor = NMColor(255, 100, 255, 0.4)
            shapeCircle.relative = false
            shapeCircle.borderStyle = NMLineStyle.DOT
            shapeCircle.radius = 500.0
            shapeCircle.x = 4000.0
            shapeCircle.y = 4000.0

            // Create 2 layer called layerA and layerB and add to them the markers
            mapview.addLayer("layerA")
            mapview.addMarker("layerA", marker)
            mapview.addPath("layerA", path)
                
            mapview.addLayer("layerB")
            mapview.addShape("layerB", shape)
            mapview.addShape("layerB", shapeCircle)

            // Call apply to see all changes
            mapview.apply()

            // Hide layerB after 5seconds, and show it again after 10seconds
            GlobalScope.launch(Dispatchers.Main){
                delay(5000L)

                mapview.setLayerVisibility("layerB", false)
                mapview.apply()

                delay(5000L)
                mapview.setLayerVisibility("laberB", true)
                mapview.apply()
            }
        }
    }
    ```
=== "iOS"
    ```swift
    // In your AppDelegate
    class AppDelegate: UIResponder, UIApplicationDelegate {
        ...
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
            ...
            NextomeMapViewHandler.instance.initialize()
            ...
            return true
        }
        ...
    }

    // In your UIViewController
    import NextomeMapView

    class ViewController: UIViewController {
        
        override func viewDidLoad() {
            super.viewDidLoad()

            var vc = NextomeMapViewHandler.instance.initializeFlutterViewController()
            self.present(vc, animated: true)
            
            var tile: NMTile = NMTile(name: "Tile1", id: "1234", source: "PATH TO THE .ZIP")
            NextomeMapViewHandler.instance.setResources(tiles: [tile], zoom: 1, width: 12000, height: 6500)
            
            // Subscribe to evetns                
            NextomeMapViewHandler.instance.setOnMapReady(callback: mapReady)
            NextomeMapViewHandler.instance.setOnMapTap(callback: mapTap)
            NextomeMapViewHandler.instance.setOnMapLongPress(callback: mapLongPress)
            NextomeMapViewHandler.instance.setOnMarkerTap(callback: markerTap)
        }

        // EVENTS -------------->        
        func mapReady() -> Void {
            print("CALLBACK MAP IS READY")
            populate()
        }
        
        func mapTap(x: Double, y: Double) -> Void {
            print("CALLBACK MAP TAP \(x) \(y)")
        }
        
        func mapLongPress(x: Double, y: Double) -> Void {
            print("CALLBACK MAP LONG PRESS \(x) \(y)")
        }
        
        func markerTap(marker: NMMarker) -> Void {
            print("CALLBACK MARKER TAP ON \(marker.id ?? "ID IS NULL")")
        }
        // <------------------------
        
        func populate() -> Void {

            // Marker placed at position 1000, 1000
            var marker: NMMarker = NMMarker()
            marker.id = "image_id"
            marker.x = 1000.0
            marker.y = 1000.0
            marker.height = 30.0
            marker.width = 30.0
            marker.source = "PATH TO IMAGE FILE"
            marker.sourceType = NMSourceType.FILESYSTEM

            // Path described by 2 points from 1000,1000 to 3000,3000
            var path: NMPath = NMPath()
            path.color = NMColor(r: 255, g: 100, b: 100)
            path.width = 2.0;
            path.style = NMLineStyle.DOT
            path.id = "path_id"
            path.points = [NMPoint(x: 1000.0, y: 1000.0), NMPoint(x: 3000.0, y: 3000.0)]

            // Shape like triangle described by 3 points 
            var shape: NMShape = NMShape()
            shape.id = "shape_1_id"
            shape.borderWidth = 3.0
            shape.borderColor = NMColor(r: 100, g: 255, b: 255)
            shape.fillColor = NMColor(r: 100, g: 100, b: 255, a: 0.23)
            shape.relative = false
            shape.points = [NMPoint(x: 1300.0, y: 1500.0), NMPoint(x: 3500.0, y: 3500.0), NMPoint(x: 1300.0, y: 3500.0)]

            // Shape described as circle setted by radius
            var shapeCircle: NMShape = NMShape()
            shapeCircle.id = "shape_2_id"
            shapeCircle.borderWidth = 3.0
            shapeCircle.borderColor = NMColor(r: 100, g: 255, b: 100)
            shapeCircle.fillColor = NMColor(r: 255, g: 100, b: 255, a: 0.4)
            shapeCircle.relative = false
            shapeCircle.borderStyle = NMLineStyle.DOT
            shapeCircle.radius = 500.0
            shapeCircle.x = 4000.0
            shapeCircle.y = 4000.0

            // Create 2 layer called layerA and layerB and add to them the markers
            NextomeMapViewHandler.instance.addLayer(layerId: "layerA")
            NextomeMapViewHandler.instance.addMarker(layerId: "layerA", marker: marker)
            NextomeMapViewHandler.instance.addPath(layerId: "layerA", path:path)

            NextomeMapViewHandler.instance.addLayer(layerId: "laberB")
            NextomeMapViewHandler.instance.addShape(layerId: "laberB", shape: shape)
            NextomeMapViewHandler.instance.addShape(layerId: "laberB", shape: shapeCircle)

            // Call apply to see all changes
            NextomeMapViewHandler.instance.apply()
            
            // Hide layerB after 5seconds, and show it again after 10seconds

            DispatchQueue.main.asyncAfter(deadline: .now() + 5) {
                NextomeMapViewHandler.instance.setLayerVisibility(layerId: "layerB", show: false)
                NextomeMapViewHandler.instance.apply()
            }
            
            DispatchQueue.main.asyncAfter(deadline: .now() + 10) {
                NextomeMapViewHandler.instance.setLayerVisibility(layerId: "layerB", show: true)
                NextomeMapViewHandler.instance.apply()
            }
        }

    }


    ```