## Force a specific venue
It is possible to constraint the localization only to a predetermined venue.
During the SDK Initialization, specify the `forcedVenueId`, this way the SDK will remain in `SearchVenueState` until it localize the phone inside the venue. Other venues will be ignored and if detected the SDK will return to `SearchVenueState`

=== "Android"
    ```kotlin
    nextomeSdk = NextomeLocalizationSdk(
    clientId = CLIENT_ID,
    clientSecret = CLIENT_SECRET,
    context = context as ApplicationContext,
    forcedVenueId = FORCED_VENUE_ID
    )
    ```
=== "iOS"
    ```swift
    nextomeSdk = NextomeLocalizationSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET)
    .setForceVenueId(venueId: myVenueId)
    .build()
    ```

!!! note
    If you want to always show a specific indoor map and enable the localization only for a specific venue you can combine `Initial resources` and `Force specific venue` functionality.
    Use `getVenueResources` to retrieve the image resources to  initialize the map and use the callback from the `Getting started` section to react on position updates and floor changes.