## Load initial resources
Nextome SDK supports offline mode but only if the resources of the venue were downloaded at least the first time.
It is also possible to provide the SDK initial resources to let the app work offline from the start.

The zip file with the initial resources can be downloaded from the web portal <INSERT HERE HOW> and placed inside the assets folder of the app.
Than it can be passed to the SDK, during the initialization, specifying the zip name and, eventually, a version number.
Please, update the version number incrementally when updating the zip file otherwise the update will be ignored by the SDK.

1. Add the resource in the root directory of the project

    === "Android"
        ![Initial resources assets](../../assets/initialResourceAndroid.png)
         ```kotlin
            nextomeSdk = NextomeLocalizationSdk(
                clientId = CLIENT_ID,
                clientSecret = CLIENT_SECRET,
                context = context as ApplicationContext,
                initialData = AssetResource(name = "name.zip", venueId: myVenueId, version = 1))
         ```
    === "iOS"
        ![Initial resources assets](../../assets/initialResource.png)
        ```swift 
            nextomeSdk = NextomeLocalizationSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET)
                    .setInitialData(nextomeResource: AssetResource(name: "resources.zip", venueId: myVenueId, version: 2))
                    .build()
        ```

2. Initialize the SDK with an Asset Resource

=== "Android"
      ```kotlin
        nextomeSdk = NextomeLocalizationSdk(
            clientId = CLIENT_ID,
            clientSecret = CLIENT_SECRET,
            context = context as ApplicationContext,
            initialData = AssetResource(name = "name.zip", venueId: myVenueId, version = 1))
      ```
=== "iOS"
      ```swift 
            nextomeSdk = NextomeLocalizationSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET)
                    .setInitialData(nextomeResource: AssetResource(name: "resources.zip", venueId: myVenueId, version: 2))
                    .build()
      ```


If you don't want to bundle the initial data inside the `assets` folder (for example to download it from an API) you can pass the local path by using the `LocalResource` instead of `AssetResource`.

=== "Android"
    ``` kotlin
        nextomeSdk = NextomeLocalizationSdk(
        clientId = CLIENT_ID,
        clientSecret = CLIENT_SECRET,
        context = context as ApplicationContext,
        initialData = LocalResource(path = LOCAL_PATH, venueId: myVenueId,  version = 1)
        )
    ```
=== "iOS"
    ```swift
        nextomeSdk = NextomeLocalizationSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET)
        .setInitialData(nextomeResource: LocalResource(name: LOCAL_PATH, venueId: myVenueId, version: 2))
        .build()
    ```