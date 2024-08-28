# iOS Integration - Getting started

<!--A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel). Run to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using OpenStreetMap for outdoor and Nextome Flutter Map for indoor.
-->

## Prerequisites

- Xcode 15.0+
- Make sure that your project meets these requirements: 
    - Swift 5.7
    - Minimum deployment: iOS 14.0
- Have credentials for jFrog
- Have credentials for our portal

## How to include

### Cocoapods

Phoenix Sdk is distributed with [Cocoapods](https://guides.cocoapods.org/) using our Podspec repo. Be sure to have CocoaPods installed, or follow [this guide](https://guides.cocoapods.org/using/getting-started.html) to install it.

Then it is necessary to configure our Spec Repo.

1. Add credentials  in the netrc file

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

    ```bash
    pod init
    ```

6. To your Podfile, be sure that the platform is at least 13.2 then add the CocoaPods specs source and our Nextome source. Then add the PhoenixSdk pod

    ```
    platform :ios, '14.0'

    source 'https://github.com/CocoaPods/Specs.git'
    source 'https://github.com/Nextome/SpecsExperimental'

    use_frameworks!

    target 'MyApp' do
        pod 'NextomeProximity'
    end
    ```

    !!!note
        Since SDK is still in RC CocoaPods require to specify the version explicitly like this
        ```swift
            pod 'NextomeProximity', '1.0.0-rc1'
        ```


7. Install the pods, then open your .xcworkspace file to see the project in Xcode

    ```bash
    pod install
    ```

8. Open your `xcworkspace` file


## Setup

In order to work properly the SDK requires to setup some permissions and capabilities:

### Add Background capabilities
If your app needs to compute the position even if it is in background, it is required to add the background capability.

1. Select your project in Xcode’s Project navigator.
2. Select the app’s target in the Targets list.
3. Click the Signing & Capabilities tab in the project editor.
4. Add the background capability

    ![Add Capability](../../assets/Proximity/iOS/addCapabilities.png)

5. Add Location Updates, Background Fetch and Background Processing capabilities

    ![Background capability](../../assets/Proximity/iOS/backgroundCapability.png)

 
### Add permissions

Add these permissions in the `info.plist`:

1. Privacy - Bluetooth Always Usage Description
2. Privacy - Location Always and When In Use Usage Description
3. Privacy - Location Always Usage Description
4. Privacy - Location When In Use Usage Description
5. Permitted background task scheduler identifiers -> com.nextome.sdk.proximity.bgranging

### Retrive SDK Credentials
Log-in the web dashboard and retrieve the `Client` and `Secret Key` for the SDK.
Those credentials are available from your profile, in the Apps section. 

![Retrieve SDK Credentials](../../assets/sdk_key.png)

## SDK Initialization


First import the Proximity SDK Module

```swift
import NextomeProximity
```

Then initialize the NextomeProximitySdk. 
Initialize the Nextome Proximity SDK inside ```application(_:didFinishLaunchingWithOptions:)``` method in your AppDelegate class like below.
Do that inside didFinishLaunchingWithOptions method of AppDelegate, allow the application to register correctly the listener of beacons and call delegates methods on wake up of the application
if it has been killed.

It requires the given `Client` and `Secret Key`.

```swift
...
import NextomeProximity
...

@UIApplicationMain
class AppDelegate: UIApplicationDelegate { 
    ...
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]?) -> Bool { 
        ...
        NextomeProximitySdk.shared.instance.initialize(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET)
        NextomeProximitySdk.shared.instance.getInteractionsObservables().watch(block: { interaction in
            // Got interaction, do something like notifiy at the user
        })
        ...       
    }
    ...
}
```

!!!note
    By default the SDK works with settings defined in the web portal.
    The NextomeProximitySdk allows to override some of those as described in the next sections.
    But please notice that this operation is extremely dangerous and should only be made in accordance with the Nextome Team because has an huge impact on the proximity's performance.


!!!warning
    If during build phase you encounter an error like this ```error: Sandbox: rsync.samba(86184) deny(1) file-write-create /Users/...../Frameworks/SSZipArchive.framework/SSZipArchive.bundle```, then go to build settings of your project, search for ENABLE_USER_SCRIPT_SANDBOXING property and set it to 'NO'


## Next steps
- See [Start Proximity](../Basic%20Features/start-proximity.md) to use Nextome SDK.

## Examples
A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel).


<br>

**© 2023 Nextome srl | All Rights Reserved.**