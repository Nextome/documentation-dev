# Android Integration - Getting started

A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel). Run the MapActivity to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using OpenStreetMap for outdoor and Nextome Flutter Map for indoor.

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
                    url "https://nextome.jfrog.io/artifactory/nextome-libs-prod/"

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
                    url = uri("https://nextome.jfrog.io/artifactory/nextome-libs-prod/")

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
        implementation 'net.nextome.phoenix:sdk:{last_version}'
        ```

    === "KTS"

        ``` kotlin title="project/build.gradle.kts"
        implementation ("net.nextome.phoenix:sdk:{last_version}")
        ```
    Check latest released version [here](/docs/Nextome%20SDK/Android/changelog.md)

## Required permissions
To run, Nextome SDK requires the following permissions:
```xml title="AndroidManifest.xml"
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    
    <!-- needed to retrieve GPS position when outdoor -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    
    <!-- needed to scan and connect to beacons -->
    <uses-permission android:name="android.permission.BLUETOOTH"
                     android:maxSdkVersion="30" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"
                     android:maxSdkVersion="30" />
    
    <uses-permission android:name="android.permission.BLUETOOTH_SCAN" />
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE" />
    
    <!-- needed for background localization -->
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
```

!!!note 
    The app integrating Nextome needs to ask the appropriate permissions and make sure they are accepted by the user.

## SDK Initialization
It is possible to access all the methods of Nextome using the class `NextomePhoenixSdk`.
It requires the `application context`, the given `Client` and `Secret Key`.

!!!note
    It is possible to generate or invalidate a given Client and Secret Key using our [web frontend](#retreive-client-and-secret-key).

```kotlin
    nextomeSdk = NextomePhoenixSdk(
        clientId = CLIENT_ID,
        clientSecret = CLIENT_SECRET,
        context = context as ApplicationContext,
    )
```

!!!warning
    By default the SDK works with settings defined on the web frontend.<br><br>
    If you know what you are doing, you can override those settings as described [here](Android/settings.md).
    However, we strongly suggest to consult Nextome team before, since they can
    degrade sdk performances and cause phone battery drain.

## Next steps

- See [Start Localization](../start-localization.md) to use Nextome SDK.

## Examples
A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel).
Run the `MapActivity` to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using *OpenStreetMap* for outdoor and *Nextome Flutter Map* for indoor.

<br>

**© 2023 Nextome srl | All Rights Reserved.**
