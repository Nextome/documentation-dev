# Android Integration - Getting started

<!--A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel). Run the MapActivity to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using OpenStreetMap for outdoor and Nextome Flutter Map for indoor.-->

## Prerequisites
- Your project has min SDK version >= 23;
- Have working credentials for our artifactory repository;
- Have working credentials for our [frontend portal](https://admin.nextome.net/);

!!! warning "Credentials"
    If you need access to artifactory or web frontend, contact us at [info@nextome.com](mailto:info@nextome.com).

### Retreive Client and Secret Key
Log-in the web dashboard and retrieve the `Client` and `Secret Key` for the SDK.
Those credentials are available from your profile, in the Apps section.

![Retrieve SDK Credentials](../../assets/sdk_key.png)

## How to include

1. Add our repositories in the Gradle Project Settings `settings.gradle.kts`:

    === "Groovy"
        ``` groovy title="settings.gradle"
        
        dependencyResolutionManagement {
            repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
            repositories {
                ...

                google()
                mavenCentral()
                maven {
                    url "https://packages.nextome.dev/artifactory/nextome-sdk-experimental-android-local/"

                    credentials {
                        username "USERNAME"
                        password "PASSWORD"
                    }
                }
            }
        }
        ```
    === "KTS"
        ``` kotlin title="settings.gradle.kts"
    
        dependencyResolutionManagement {
            repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
            repositories {
                ...

                google()
                mavenCentral()
                maven {
                    url "https://packages.nextome.dev/artifactory/nextome-sdk-experimental-android-local/"

                    credentials {
                        username = "USERNAME"
                        password = "PASSWORD"
                    }
                }
            }
        }
        ```

2. In your module (app-level) Gradle file, add the dependency for the SDK:

    === "Groovy"

        ``` groovy title="project/build.gradle"
        implementation 'com.nextome:nextome_proximity:{last_version}'
        ```

    === "KTS"

        ``` kotlin title="project/build.gradle.kts"
        implementation ("com.nextome:nextome_proximity:{last_version}")
        ```
    Check latest released version [here](../Android/changelog.md)

## Required permissions
To run, Nextome SDK requires the following permissions:
```xml title="AndroidManifest.xml"
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!-- needed to scan and connect to beacons -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.BLUETOOTH_SCAN" />
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />

    <!-- needed for background monitoring -->
    <uses-permission android:name="android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS" />
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_LOCATION" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_DATA_SYNC" />
```

!!!note 
    The app integrating Nextome needs to ask the appropriate permissions and make sure they are accepted by the user.
    If permissions are not granted, the SDK will not works properly.

## SDK Initialization
It is possible to access all the methods of Nextome using the class `NextomeProximitySdk`.
It requires the `application context`, the given `Client` and `Secret Key`.

!!!note
    It is possible to generate or invalidate a given Client and Secret Key using our [web frontend](#retreive-client-and-secret-key).

Initialize the Nextome Proximity SDK inside ```onCreate``` method on your **Application** class like below.
Do that inside ```onCreate``` method of Application allows the application to register correctly the listener of beacons and call deletage methods on the wake up of the application
if it has been killed.

!!!note
    It is very important to initialize the SDK inside onCreate method of the **Application** extended class and not in an Activity or an AppCompatActivity extension.
    Also assign if not is assigned yet, the attribe **android:name** at application tag of your AndroidManifest.xml with your application name.
    ```
        <application
            android:name=".DemoProximityApplication"
        ...
    ```

```kotlin

...
import com.nextome.proximity.NextomeProximitySdk
import com.nextome.sdk.androidproximityapp.DiModules.appModule
...

class DemoProximityApplication: Application() {
    override fun onCreate(){
        ...
        NextomeProximitySdk.instance.initialize(CLIENT_ID, CLIENT_SECRET)
        GlobalScope.launch {
            NextomeProximitySdk.instance.getInteractionsObservables()?.collect {
                var interaction: NextomeInteraction = it
                // Do something with interaction like sending an in-app notification to the user with interaction data
            }
        }
        ...
    }
}
```

## Next steps

- See [Start Proximity](../Basic%20Features/start-proximity.md) to use Nextome Proximity SDK.

<!--## Examples
A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel).
<br>-->

**© 2024 Nextome srl | All Rights Reserved.**
