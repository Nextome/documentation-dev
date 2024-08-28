# Basic Features

<!--A full working example app is available for [iOS](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel) and [android](https://github.com/Nextome/nextome-phoenix-android-whitelabel). Run the MapActivity to see Nextome Sdk in action. -->

## Start proximity service
To start Proximity, call:

=== "Android"
    ```kotlin
    val venues: List<Int> = listOf(YOUR_VENUE_ID_1, YOUR_VENUE_ID_2)
    NextomeProximitySdk.instance.startService(venues) {
        // Service started
    }
    ```
=== "iOS"
    ```swift
    let venues: [KotlinInt] = [YOUR_VENUE_ID_1, YOUR_VENUE_ID_2] 
    NextomeProximitySdk.shared.instance.startService(venues: venues){
        // Service started
    }
    ```
The  ```startService ``` method accept a list of integers that represent the venues id where you want to start the proximity service. After service is started, a callback will be returned. 

!!!tip "Background Mode"
    The proximity service start must be called when application is in foreground mode.

## Stop proximity
When you want to stop proximity service, call:

=== "Android"
    ```kotlin
    NextomeProximitySdk.instance.stopService {
        // Service stopped
    }
    ```
=== "iOS"
    ```swift
    NextomeProximitySdk.shared.instance.stopService(){
        // Service stopped
    }
    ```
    !!!tip "Background Mode"
        The proximity service will be stopped on all venues where proximity services has been started.

## Observe for interaction trigger event
It's possible to observe when an interaction has been fired by the Proximity SDK.
Visit [Interaction](./interaction.md) to know more about interaction object.

=== "Android"
    ```kotlin
    val state: Flow<NextomeSdkState> = nextomeSdk.getStateObservable()
    yourScope.launch {
        NextomeProximitySdk.instance.getInteractionsObservables()?.collect {
            // The interaction is fired by the SDK. Do something.      
            var interaction: NextomeInteraction = it          
        }
    }
    ```
    !!!tip "Observeble into a scope"
        To avoid concurrency problem, ```getInteractionsObservables``` method need to live inside a coroutine scope.

    !!!arning "Observeble at application start"
        Is strongly recommanded to call this method at Application.onCreate method as shown at [Getting Started]("../../Getting Started/android-getting-started.md") section to be sure that a Enter/Exit region event can be correctly dispatched by operative system to the application.

=== "iOS"
    ```swift
    NextomeProximitySdk.shared.instance.getInteractionsObservables().watch(block: { interaction in
        // The interaction is fired by the SDK. Do something. 
    })
    ```

## Get observed venues
It's possible get a list of venues id where proximity service is on. That's is hopeful when you open the application again after service has been started. This method allow you to know which venues, so which interactions, the application is listening for.

=== "Android"
    ```kotlin
    val observed: List<Int> = NextomeProximitySdk.instance.getObservedVenues()
    ```
=== "iOS"
    ```swift
    var observed: [Int] = []
    NextomeProximitySdk.shared.instance.getObservedVenues().forEach{ venueId in
        observed.append(venueId.intValue)
    }
    ```

<!--
## Next steps

- Visit [Background-service](Android/background-service.md) to learn how to use the SDK even when the app is not opened in the foreground.
-->

<!--Visit [Nextome map integration](nextome-map-integration.md) if you want to use our library to display the indoor map.-->

<!--## Examples
A full working example app is available on [iOS](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel) and [android](https://github.com/Nextome/nextome-phoenix-android-whitelabel).
Run the `MapActivity` to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using *OpenStreetMap* for outdoor and *Nextome Flutter Map* for indoor.
-->

<br>

**© 2024 Nextome srl | All Rights Reserved.**
