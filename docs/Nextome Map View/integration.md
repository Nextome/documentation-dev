# Integrate Nextome MapView

<!--- ## Flutter integraton

To integrate the Nextome Map View you need to add the following code in your `pubspec.yaml` Flutter project and run the command `flutter pub get`

```yaml
    dependencies:
        # other your dependencies
        nextome_map_module:
            git: 
                url: https://github.com/Nextome/flutter_mapview.git
                ref: newmapview
```

-->

## Android integraton

### Prerequisites
- Your project has min SDK version >= 23
- Have working credentials for our Artifactory repository

!!! warning "Credentials"
    If you need access to artifactory, contact us at [info@nextome.com](mailto:info@nextome.com).

### How to include

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
                    url "https://storage.googleapis.com/download.flutter.io"
                }
                maven {
                    url "https://packages.nextome.dev/artifactory/nextome-libs-prod/"

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
                    url = uri("https://storage.googleapis.com/download.flutter.io") 
                }
                maven {
                    url = uri("https://packages.nextome.dev/artifactory/nextome-libs-prod/")

                    credentials {
                        username = "USERNAME"
                        password = "PASSWORD"
                    }
                }
            }
        }
        ```

2. In your module (app-level) Gradle file, add the dependency for the NextomeMapView:

    === "Groovy"

        ``` groovy title="project/build.gradle"
        implementation 'com.nextome.nextomemapview:nextomemapview:{last_version}'
        ```

    === "KTS"

        ``` kotlin title="project/build.gradle.kts"
        implementation ("com.nextome.nextomemapview:nextomemapview:{last_version}")
        ```
    Check latest released version [here](/docs/Nextome%20SDK/Android/changelog.md)

### Required permissions
To run, NextomeMapView requires the following permissions:
```xml title="AndroidManifest.xml"
    <!-- needed for map file usage -->
    <uses-permission android:name="android.permission.MANAGE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

### Next steps

- See [Map-usage](./Usage/initialize.md) to use Nextome MapView component.

### Examples
A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-android-whitelabel).
Run the `MapActivity` to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using *OpenStreetMap* for outdoor and *Nextome Flutter Map* for indoor.

<br>

## IOS integraton

### Prerequisites

- Xcode 14.3
- Make sure that your project meets these requirements: 
    - Swift 5.7
    - Minimum deployment: iOS 13.2
- Have working credentials for our Artifactory repository

!!! warning "Credentials"
    If you need access to artifactory, contact us at [info@nextome.com](mailto:info@nextome.com).

### How to include

#### Cocoapods

NextomeMapView is distributed with [Cocoapods](https://guides.cocoapods.org/) and artifactory. Be sure to have CocoaPods installed, or follow [this guide](https://guides.cocoapods.org/using/getting-started.html) to install it.

Then it is necessary to configure our private Spec Repo.

1. Add artifactory credentials in the netrc file

    ``` bash 
    nano ~/.netrc
    ```
    Copy this then close and save

    ```
    machine packages.nextome.dev
    login <USERNAME>
    password <ENCRYPTED-PASSWORD>
    ```

    !!! note
        If you get an error of this type: "Permission bits, should be 0600, but are 644"
        
        Run this command: 

        `chmod 0600 ~/.netrc`
     

2. Create a Podfile if you don't already have one. From the root of your project directory, run the following command
    ``` bash 
    pod init
    ```

3. To your Podfile, be sure that the platform is at least 13.2 then add the CocoaPods specs source and our Nextome source. Then add the NextomeMapView pod

    As opposed to the cocoapods client's default behavior, the cocoapods-art plugin does not automatically update its index whenever you run client commands (such as install). To keep your plugin's index synchronized with your CocoaPods repository, you need to update it by executing the following command:

    ```bash
    platform :ios, '13.2'

    source 'https://github.com/CocoaPods/Specs.git'TODO
    source 'https://github.com/Nextome/Specs'

    use_frameworks!

    target 'MyApp' do
        pod 'NextomeMapView', '{last_version}'
    end
    ```

    !!! note
        Since NextomeMapView is still in RC CocoaPods require to specify the version explicitly like this
        `pod 'NextomeMapView', '2.0.0-rc1'`

4. Install the pods, then open your .xcworkspace file to see the project in Xcode

    ```bash
    pod install
    ```

5. Open your `xcworkspace` file and start to use NextomeMapView

### Next steps

- See [Map-usage](./Usage/initialize.md) to use Nextome MapView component.