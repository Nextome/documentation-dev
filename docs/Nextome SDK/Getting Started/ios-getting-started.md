# iOS Integration - Getting started

A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel). Run to see Nextome Sdk in action. It also contains a seamless outdoor/indoor map integration using OpenStreetMap for outdoor and Nextome Flutter Map for indoor.

## Prerequisites

- Xcode 15.0
- Make sure that your project meets these requirements: 
    - Swift 5.7
    - Minimum deployment: iOS 13.2
- Have credentials for jFrog
- Have credentials for our portal

## How to include

### Cocoapods

Nextome Localization Sdk is distributed with [Cocoapods](https://guides.cocoapods.org/) using our Podspec repo. Be sure to have CocoaPods installed, or follow [this guide](https://guides.cocoapods.org/using/getting-started.html) to install it.

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

6. To your Podfile, be sure that the platform is at least 13.2 then add the CocoaPods specs source and our Nextome source. Then add the NextomeLocalization pod

    ```
    platform :ios, '13.2'

    source 'https://github.com/CocoaPods/Specs.git'
    source 'https://github.com/Nextome/Specs'

    use_frameworks!

    target 'MyApp' do
        pod 'NextomeLocalization'
    end
    ```

    !!!note
        Since SDK is still in RC CocoaPods require to specify the version explicitly like this
        ```swift
            pod 'NextomeLocalization', '3.0.0-rc02'
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

    ![Add Capability](../../assets/addCapabilities.png)

5. Add background processing and Location Updates

    ![Background capability](../../assets/backroundCapability.png)

 
### Add permissions

Add these permissions in the `info.plist`:

1. Privacy - Location Always and When In Use Usage Description
2. Privacy - Location Always Usage Description
3. Privacy - Location When In Use Usage Description

### Retrive SDK Credentials
Log-in the web dashboard and retrieve the `Client` and `Secret Key` for the SDK.
Those credentials are available from your profile, in the Apps section. 

![Retrieve SDK Credentials](../../assets/sdk_key.png)

## SDK Initialization
Firsty import the Nextome Localization SDK Module
```swift
import NextomeLocalization
```

Then initialize the NextomeLocalizationSdk.

It requires the given `Client` and `Secret Key`.

```swift
let nextomeSdk = NextomeLocalizationSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET).build()
```

!!!note
    By default the SDK works with settings defined in the web portal.
    The NextomeLocalizationSdk.Builder allows to override some of those as described in the next sections.
    But please notice that this operation is extremely dangerous and should only be made in accordance with the Nextome Team because has an huge impact on the localization's performance.

## Next steps
- See [Start Localization](../start-localization.md) to use Nextome SDK.

## Examples
A full working example app is available on [this repository](https://github.com/Nextome/nextome-phoenix-iOS-whitelabel).


<br>

**© 2023 Nextome srl | All Rights Reserved.**