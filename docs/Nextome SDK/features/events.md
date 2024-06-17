# Observe events

!!!notes
    Event positions and radius can be customized on Nextome Web Frontend.

If there is at least one event created in your venue, you can observe when user enters or exits from event radius using those two observers:
=== "Android"
    ```kotlin
        nextomeSdk.getEnterEventObservable().asLiveData().observe(this) { event ->
            Log.e("event_test", "Received on enter with id ${event.event.id} and data: ${event.event.data}")            
        }
    
        nextomeSdk.getExitEventObservable().asLiveData().observe(this) { event ->
            Log.e("event_test", "Received on exit with id ${event.event.id} and data: ${event.event.data}")
        }
    ```
=== "iOS"
    ```swift
    let enterObserver = nextomeSdk.getEnterEventObservable().watch(){ event in
        guard let event = event else{
            return
        }
    
        print("Received on enter with id \(event.event.id) and data: \(event.event.data)")
    }

    let exitObserver = nextomeSdk.getEnterEventObservable().watch() { event in
        guard let event = event else{
            return
        }
        print("Received on exit with id \(event.event.id) and data: \(event.event.data)")
    }
    ```

Optionally, after exiting from event, it's possible to set a timeout value in seconds before receiving the same event again.
This allows you to ignore when user exits and enters again in an event radius in a short time span.

For example, if timeout is set to 60 seconds, the same event onEnter after onExit is received will be not triggered again for at least for 1 minute.
If set to 0, Nextome SDK will signal onEnter and onExit event realtime.

This parameter is only configurable during the SDK initialization.

=== "Android"
    ``` kotlin
    nextomeSdk = NextomeLocalizationSdk(
        clientId = CLIENT_ID,
        clientSecret = CLIENT_SECRET,
        context = context as ApplicationContext,
        eventTimeoutDurationInSeconds = 10
    )
    ```
=== "iOS"
    ``` swift
    NextomeLocalizationSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET)
        .setEventTimeoutDuration(millis: 60)
        .build()
    ```

## Query past events
It is possible to query recent events using
=== "Android"
    ```kotlin
    val todayEvents: List<NextomeEventEnterExit> = nextomeSdk.getEnterExitEventsAfter(ts = todayAtMidnightInMillis)
    ```
=== "iOS"
    ```swift
    let todayEvents = nextomeSdk.getEnterExitEventsAfter(ts: todayAtMidnightInMillis)
    ```

## Example
Events can enable some custom logic in the client application. For example, it's possible to use events to
count all the people that are entering an area.

!!!note
    Each event can deliver a custom payload (for example a JSON). To customize Event data, use Nextome Web Frontend.

=== "Android"
    ```kotlin
    data class EventData(
        val roomId: Int,
        val roomName: String
    )

    nextomeSdk.getEnterEventObservable().asLiveData().observe(this) {
        val eventData: EventData = it.event.data.parseFromJson()
        val timestamp: Long = it.ts
        
        api.sendEnterEvent(roomId = eventData.roomId, ts = timestamp)
        showMessage("Welcome in room ${eventData.roomName}")
    }

    nextomeSdk.getExitEventObservable().asLiveData().observe(this) { event ->
        val eventData: EventData = it.event.data.parseFromJson()
        val timestamp: Long = it.ts

        api.sendExitEvent(roomId = event.roomId, ts = timestamp)
        showMessage("You're now leaving room ${eventData.roomName}")
    }
    ```
=== "iOS"
    ```swift
    enterObserver = nextomeSdk.getEnterEventObservable().watch{event in
            guard let event = event, let eventPayload = event.event.data else { return }
            
            let eventData: EventData = eventPayload.parseFromJson()
            let timestamp = event.ts
            
            api.sendEnterEvent(forRoom: eventData.roomId, at: timestamp)
            showMessage("Welcome in room \(eventData.roomName)")
            
        }
        
    exitObserver = nextomeSdk.getExitEventObservable().watch{ event in
        guard let event = event, let eventPayload = event.event.data else { return }
        
        let eventData: EventData = eventPayload.parseFromJson()
        let timestamp = event.ts
        
        api.sendExitEvent(forRoom: eventData.roomId, at: timestamp)
        showMessage("You're now leaving room \(eventData.roomName)")
    }
    ```