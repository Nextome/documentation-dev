---
title: Nextome map integration
hide:
  - footer
---
# Nextome map integration
If you want, there’s an optional module that can show a live map of the indoor location of the user. 
You have two options to integrate the UI component:

1. Integrate the `NextomeLocalizationMapUtils` (Recommended method), which is a wrapper that allows you to integrate easily the SDK with the Map module
2. Integrate directly the `NextomeMap`. This method requires more boilerplate code but allows more customization

<p style="text-align: center;"><img src="/assets/mapIos.jpeg" width="50%"></p>

## Integrate NextomeLocalizationMapUtils
In this sections we will go through the integration of the NextomeLocalizationMapUtils. If you're interested in th NextomeMap approach you can refer to this documentation.

### Install the dependency

=== "Android"
    1. In your `build.gradle` file, add dependencies for Nextome MapView and NextomeLocalizationMapUtils:
    ```kotlin
        implementation("com.nextome.localization_map_utils:nextome_localization_map_utils:1.5.1.1")
        implementation("net.nextome.nextome_map_module:flutter:1.5.1")
    ```

=== "iOS"
    1. Follow the [How to include steps](https://docs.nextome.dev/Nextome%20SDK/Getting%20Started/ios-getting-started/#how-to-include) 
    2. Update the `Podfile` adding the CocoaPods source and the Nextome source, the pod dependency and the post install script. You don't need to specify the Nextome Localization sdk because it is already defined as NextomeLocalizationMapUtils dependency.
    ```swift
    platform :ios, '13.2'

    source 'https://github.com/CocoaPods/Specs.git'
    source 'https://github.com/Nextome/Specs'

    use_frameworks!

    target 'MyApp' do
        pod 'NextomeLocalizationMapUtils_Release', '1.4.4.2'
    end

    post_install do |installer|
        installer.generated_projects.each do |project|
            project.targets.each do |target|
                target.build_configurations.each do |config|
                    config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '13.2'
                end
            end
        end
    end
    ```

        !!! note
            The NextomeLocalizationUtils is distributed in two different pod version, `Release` and `Debug`. Th release version will compile also for the simulate but will not show the map view. To test the map on the simulator you can use the `Debug` version instead. 
            It is important to use the `Release` version for the app store. 

    4. Install the dependency
        ```swift
        pod install
        ```

### Initialization 

The first step is to initialize the module.

=== "Android"
    1. In your .xml layout, add a `NextomeLocalizationMapView` where you want to display the indoor map.
    ```xml
        <com.nextome.nextome_localization_map_utils.NextomeLocalizationMapView
            android:id="@+id/indoor_map"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    ```
    2. In your Activity/ViewModel, initialize `NextomeLocalizationMapHandler`
    ```kotlin
        val handler = NextomeLocalizationMapHandler()

        val fragmentManager: FragmentManager = supportFragmentManager
        handler.initialize(
            fragmentManager = fragmentManager,
            viewId = R.id.indoor_map, // View created before
            context = this
        )
    ```

=== "iOS"
    1. Open th `AppDelegate`
    2. Import the module
        ```swift
            import NextomeLocalizationMapUtils
        ```
    3. Initialize the `NextomeLocalizationMapHandler` in the `didFinishLaunchingWithOptions` method:
        ```swift
            func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
                // Override point for customization after application launch.
        NextomeLocalizationMapHandler.instance.initialize()
                return true
            } 
        ```
    The `NextomeLocalizationMapHandler` provide a UIViewController/Fragment and some methods to handle it.
    ### Integration
    1. Import the module
        ```swift
            import NextomeLocalizationMapUtils
        ```
    2. Initialize the Fragment/UIViewController
    The `NextomeLocalizationMapHandler` provides a UIViewController that you can use to show the flutter map.

    ```swift
        lazy var indoorMapViewController: UIViewController = NextomeLocalizationMapHandler.instance.initializeFlutterViewController()
    ```

### Show a new indoor map

Once a map is available, pass the `tiles local directory`, `height` and `width` of the map to the `NextomeLocalizationMapHandler`.
Those parameters can be obtained from the `LocalizationRunningState` during localization or from the specific map returned in the `NextomeVenueData`. More info on the venue data can be retrieve [here](features/venue-data.md).
=== "Android"
    `handler.setMap(mapTilesUrl = mapTilesUrl, mapHeight = mapHeight, mapWidth = mapWidth)`

=== "iOS"
    ```swift
    NextomeLocalizationMapHandler.instance.setMap(tilesZipPath: runningState.tilesZipPath,
                                        mapHeight: runningState.mapHeight,
                                        mapWidth: runningState.mapWidth)
    ```

### Update user live position

When a new indoor user position is available, notify `NextomeLocalizationMapHandler` to update the blue point with:

=== "Android"
    ```kotlin
        viewModel.nextomeSdk.getLocalizationObservable().asLiveData().observe(this) {
            handler.updatePositionOnMap(it)
        }    
    ```

=== "iOS"
    ```swift
        func observeSdkLocations(){
        let watcher = nextomeSdk.getLocalizationObservable().watch(block: {position in
            if let position = position{
                self.lastPosition = position
                NextomeLocalizationMapHandler.instance.updatePositionOnMap(position)
            }
        })
    
        watchers.append(watcher)
        }   
    ```

### Show a list of Point of Interest
You can set the list of Point of Interest to show on the map using this method:
   
=== "Android"
    ```kotlin
    handler.updatePoiList(pois)
    ```

=== "iOS"
    ```swift
    let pois: [NextomePoi] = runningState.venueData.getPoisByMapId(mapId: runningState.mapId)
    NextomeLocalizationMapHandler.instance.updatePoiList(pois)
    ```

### Show a path between two points on map
You can show a path on the map using this method and passing a list of Vertex. You can automatically get the Vertext between a start and end point of the map using the appropriate method on the [nextomeSdk](features/navigation.md):

=== "Android"
    ```kotlin
    hanlder.updatePath(path)
    ```

=== "iOS"
    ```swift
    func showPath(path: [Vertex]){
    NextomeLocalizationMapHandler.instance.updatePath(path: path)
    }
    ```

### Observe events on Flutter Map
With this observer you can be notified of events on the map, for example, when the user decides to navigate to a POI.

=== "Android"
    ```kotlin 
    handler.observeEvents().collect { event ->
        when (event) { 
            is NextomeLocalizationMapHandler.OnNavigationSelected -> {
                // user clicked on "Navigate to POI"
                val poi = event.poi
            } 
            is NextomeLocalizationMapHandler.OnPoiClicked -> { 
                // user clicked on poi 
                val poi = event.poi 
            }
        } 
    }
    ```

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user clicked on a POI.

        ```swift
        func onPoiClicked(poi: NextomePoi){

        }
        ```
    3. Implement the onNavigationSelected method to be notified when the user wants to navigate to a POI.

        ```swift
        func onPoiClicked(poi: NextomePoi){

        }
        ```

### Show Center Position Fab
It’s possible to show an optional fab at the bottom right of the map. When clicked, the button will:

- center the map on the user position;
- follow the user position live on map;
- rotate the map based on user compass;

To enable the button, use:

=== "Android"
    `handler.setMapViewSettings(fabEnabled = true)`

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user want to navigate to a certain POI.

        ```swift
        NextomeLocalizationMapHandler.instance.setMapSettings(fabEnabled: true)
        ```

### Change user position icon
The default position icon is a blue dot. If you want to change the icon, you can load a remote resource from an url.
=== "Android"
    `handler.setMapViewSettings(customPositionResourceUrl = "url")`
    !!! note
        The compass feature will only work if the app has the following permissions:
        ``` 
        android.permission.INTERNET
        android.permission.ACCESS_COARSE_LOCATION
        android.permission.ACCESS_FINE_LOCATION
        ```
        If those permissions are not denied, the navigation button will only follow user position without rotating.

=== "iOS"
    1. Extend the `NextomeMapDelegate`

        ```swift
        class MapViewController: UIViewController, NextomeMapDelegate {

        }
        ```
    
    2. Implement the onPoiClicked method to be notified when the user want to navigate to a certain POI.

        ```swift
        let remotePath = ""http://nextome.com/position-icon.png""
        NextomeLocalizationMapHandler.instance.setMapSettings(customPositionResourceUrl: remotePath)
        ```


### React to activity lifecycle (Android Only)
:octicons-tag-24: NextomeLocalizationMapUtils 1.4.3.3

On Android, we suggest to foward to NextomeLocalizationMapUtils all the methods releated to the management of the activity lifecycle.
This will allow to save and recover the state of the map if configuration changes happens (for example, when the user rotates the phone screen).

To do so, override the following methods in your activity:
```kotlin
    fun onPostResume() {
        super.onPostResume()
        handler.onPostResume()
    }

    fun onNewIntent(intent: Intent) {
        handler.onNewIntent(intent)
    }

    fun onBackPressed() {
        handler.onBackPressed()
    }

    fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {
        handler.onRequestPermissionsResult(
            requestCode,
            permissions,
            grantResults
        )
    }

    fun onActivityResult(
        requestCode: Int,
        resultCode: Int,
        data: Intent?
    ) {
        super.onActivityResult(requestCode, resultCode, data)
        handler.onActivityResult(
            requestCode,
            resultCode,
            data
        )
    }

    fun onUserLeaveHint() {
        handler.onUserLeaveHint()
    }

    fun onTrimMemory(level: Int) {
        super.onTrimMemory(level)
        handler.onTrimMemory(level)
    }
```

Additionally, you must add this line in `AndroidManifest.xml` to all the activities that use NextomeMap:
```xml
    <activity
        android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale|screenLayout|density"
        ...
```
