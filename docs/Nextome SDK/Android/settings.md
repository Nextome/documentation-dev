# Override default settings
The NextomeLocalizationSdk uses some parameter to customize the algorithm and additional features. These parameter are defined in the web portal and should only be changed from there.

However, durning the NextomeLocalizationSdk initialization, it is possible to override some of these settings, the override will be forced on all the venues.

The parameters are summarized in the table below, with an indication of the default values used by the web dashboard.
!!! danger

    Overriding these parameters is a dangerous operation that should only be made in accordance with the Nextome Team because has an huge impact on the localization's performance.  

!!! note
    The longer you wait between scans, the longer it will take to detect a beacon. And the more reduce the length of the scan, the more likely it is that you might miss an advertisement from an beacon.


| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `scanPeriod: Long`   | Time in millis for the bluetooth beacon scan duration   | 100 ms |
| `betweenScanPeriod: Long`   | Time in millis to wait between bluetooth scans   | 100 ms |
| `rssiThreshold: Int`  | Ignore beacons below a custom RSSI threshold.   | -75 db |
| `beaconListMaxSize: Int`   | The SDk keeps in memory a cache of `n` RSSI entries in time for each beacon to more accurately compute user position.| 12 |
|`localizationMethod: NextomeLocalizationMethod`|Enum that sets the algorithm for localization. Choices are `LINEAR_SVD, NON_LINEAR_DA, PARTICLE`| `LINEAR_SVD`
| `particleActive: Boolean`  | Boolean that indicates if the particle filter is enabled or not | true |
| `eventTimeoutDurationInSeconds: Int`  | This allows you to ignore when user exits and enters again in a event radius in a short time span. For example, if timeout is set to 60 seconds, the same event onEnter after onExit is received will be not triggered again for at least for 1 minute | 0 (Real time) |
| `sendPositionToServer: Boolean`  | This allows you to automatically send positions real time to Nextome server and track monitor devices realtime on the web manager | false |
