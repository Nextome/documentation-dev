# Multimedia

`MultimediaModule` is the entry point to the SDK's multimedia features. It allows you to initialize, retrieve and synchronize the content associated with POIs, access locally downloaded media files, and manage progressive content loading through the `PoiContentPaginatorFacade` pagination component.

!!! note
    Before calling any of the methods described in this page, make sure the Nextome SDK has been correctly [initialized](../introduction.md) and that you have obtained an SDK instance (`nextomeSdk` in the examples below).

## Data Models

### PoiContent
Represents the content of a POI in its lightweight (preview) form.

| Field | Type | Notes |
|-------|------|-------|
| `id` | STRING | Identifier of the PoiContent |
| `names` | List\<PoiContentName\> | List of PoiContent names |
| `venueId` | LONG | Venue identifier |
| `mapId` | LONG | Map identifier |
| `poiId` | LONG | POI identifier |
| `parentId` | STRING? | Parent identifier (`null` if top-level) |
| `childrenIds` | List\<STRING\>? | List of children PoiContent IDs |
| `tagIds` | List\<STRING\>? | List of associated Tag IDs |
| `htmlUpdateAt` | LONG? | Set if an associated HtmlContent exists |
| `mediaUpdateAt` | LONG? | Set if an associated MediaContent exists |
| `sourceId` | STRING? | Source system ID |
| `sourceContentId` | STRING? | Content ID in the source system |
| `sourceLink` | STRING? | Reference link to the source system |
| `createdAt` | LONG | Creation timestamp |
| `updatedAt` | LONG? | Last update timestamp |

### PoiContentDetail
Full, detailed representation of a POI content.

| Field | Type | Notes |
|-------|------|-------|
| `id` | STRING | Identifier of the PoiContentDetail |
| `names` | List\<PoiContentName\> | List of PoiContentDetail names |
| `venueId` | LONG | Venue identifier |
| `mapId` | LONG | Map identifier |
| `poiId` | LONG | POI identifier |
| `parentId` | STRING? | Parent identifier (`null` if top-level) |
| `childrenIds` | List\<PoiContentDetail\>? | List of children `PoiContentDetail` |
| `tagIds` | List\<Tag\>? | List of associated `Tag` |
| `htmlUpdateAt` | LONG? | Set if an associated HtmlContent exists |
| `htmlContents` | List\<HtmlContent\>? | List of `HtmlContent` |
| `mediaUpdateAt` | LONG? | Set if an associated MediaContent exists |
| `mediaContents` | List\<MediaContent\>? | List of `MediaContent` |
| `sourceId` | STRING? | Source system ID |
| `sourceContentId` | STRING? | Content ID in the source system |
| `sourceLink` | STRING? | Reference link to the source system |
| `createdAt` | LONG | Creation timestamp |
| `updatedAt` | LONG? | Last update timestamp |

### MediaContent
Represents the content (PHOTO/VIDEO) of a media item belonging to a PoiContent.

| Field | Type | Notes |
|-------|------|-------|
| `id` | STRING | MediaContent identifier |
| `mediaType` | STRING | Media type (PHOTO/VIDEO...) |
| `mimeType` | STRING | Standard MIME type label |
| `mediaPath` | STRING? | Directory path |
| `createdAt` | LONG | Creation timestamp |

### HtmlContent
Represents the HTML content of a POI in its lightweight (preview) form.

| Field | Type | Notes |
|-------|------|-------|
| `id` | STRING | HtmlContent identifier |
| `locale` | STRING | HtmlContent locale |
| `categoryId` | STRING? | Set if a specific category is present |
| `htmlPath` | STRING? | Directory path |
| `updateAt` | LONG | Update timestamp |

### HtmlCategories
Represents an HTML category.

| Field | Type | Notes |
|-------|------|-------|
| `id` | STRING | HTML category identifier |
| `name` | STRING | HTML category name |
| `venueId` | LONG? | Venue the category belongs to (`null` if registered at system level) |
| `createdAt` | LONG | Creation timestamp |
| `updateAt` | LONG | Update timestamp |

### Tag
Represents a classification label.

| Field | Type | Notes |
|-------|------|-------|
| `id` | STRING | Tag identifier |
| `name` | STRING | Tag name |
| `venueId` | LONG? | Venue the Tag belongs to (`null` if registered at system level) |

### PoiContentName
Names associated with a `PoiContent`/`PoiContentDetail`.

| Field | Type | Notes |
|-------|------|-------|
| `name` | STRING | PoiContent name |
| `locale` | STRING | Name locale |

## MultimediaModule

### Initialize the multimedia module
Initializes the multimedia module using the network client provided by `NextomeLocalization`.

=== "Android"
    ```kotlin
    nextomeSdk.initMultimedia()
    ```
=== "iOS"
    ```swift
    nextomeSdk.doInitMultimedia()
    ```

!!! note
    Must be called before using any of the Multimedia module features.

### Check initialization status
Checks whether the Multimedia module has been correctly initialized. Returns `true` if the Multimedia module is initialized, `false` otherwise.

=== "Android"
    ```kotlin
    if (nextomeSdk.getMultimediaFacade().isInitialized()) {
        // Multimedia available
    }
    ```
=== "iOS"
    ```swift
    if nextomeSdk.getMultimediaFacade().isInitialized() {
        // Multimedia available
    }
    ```

!!! note
    Can be used to check the availability of the module's features before invoking them.

### Get the Multimedia facade
Returns the `MultimediaFacade` singleton instance.

=== "Android"
    ```kotlin
    val facade = nextomeSdk.getMultimediaFacade()
    ```
=== "iOS"
    ```swift
    let facade = nextomeSdk.getMultimediaFacade()
    ```

!!! note
    Returns the singleton instance currently in use by the application.

### Initialize the paginator
Creates a new `PoiContentPaginatorFacade` instance associated with the specified venue.

| Parameter | Type | Description |
|-----------|------|--------------|
| `venueId` | `Long?` | Identifier of the venue to retrieve contents for. If `null`, the repository will use its own default behavior. |

Returns a new `PoiContentPaginatorFacade` instance used to retrieve POI contents.

=== "Android"
    ```kotlin
    val paginator = nextomeSdk.getMultimediaFacade().initPaginator(100L)
    ```
=== "iOS"
    ```swift
    let paginator = nextomeSdk.getMultimediaFacade().initPaginator(venueId: 100)
    ```

!!! note
    The paginator is created in its initial state. Use `loadNextPage()` or `loadAllPages()` to start loading data. See the [PoiContentPaginatorFacade](#poicontentpaginatorfacade) section below for more details.

### Get the loaded paginator items
`getPoiPreviewItems(poiContentPaginatorFacade)` and `valueFromItemsInPaginator(poiFacade)` both return the list of `PoiContent` items currently loaded in the specified paginator, equivalent to reading `paginator.items.value` directly.

=== "Android"
    ```kotlin
    val items = nextomeSdk.getPoiPreviewItems(paginator)

    // or

    val items = nextomeSdk.valueFromItemsInPaginator(paginator)
    ```
=== "iOS"
    ```swift
    let items = nextomeSdk.getPoiPreviewItems(
        poiContentPaginatorFacade: paginator
    )
    ```

!!! note
    Useful to quickly retrieve the current list without observing the `StateFlow`.

### Observe the paginator state
`paginatorObservablePublic(facade)` converts the paginator's internal state into a public observable object for Swift/iOS. Returns a `PoiPaginatorState` exposing the loading state, items, errors and end-of-list state.

=== "iOS"
    ```swift
    let observePaginator = nextomeSdk.paginatorObservablePublic(facade: paginator)

    observePaginator.endReached.watch(block: { end in ... })
    observePaginator.isLoading.watch(block: { isLoading in ... })
    observePaginator.items.watch(block: { items in ... })
    observePaginator.error.watch(block: { error in ... })
    ```

!!! note
    All properties are exposed through observable wrappers (`wrap()`). See the [PoiContentPaginatorFacade](#poicontentpaginatorfacade) section for more details on pagination.

### Fetch POI content details
`fetchPoiContentDetails(venueId, poiContentList)` synchronizes POI contents by retrieving missing details from the server and merging them with local data.

| Parameter | Type | Description |
|-----------|------|--------------|
| `venueId` | `Long` | Identifier of the venue associated with the contents. |
| `poiContentList` | `List<PoiContent>` | List of POI contents to synchronize. |

=== "Android"
    ```kotlin
    nextomeSdk.fetchPoiContentDetails(100L, paginator.items.value)

    // or

    val list = nextomeSdk.valueFromItemsInPaginator(paginator)
    nextomeSdk.fetchPoiContentDetails(100L, list)
    ```

!!! note
    The operation is only performed if `MultimediaFacade` has been initialized. Call this once the paginator items are available.

### Get a POI content
`getPoiContent(id, venueId)` retrieves a POI content associated with a specific venue.

| Parameter | Type | Description |
|-----------|------|--------------|
| `id` | `String` | Identifier of the POI content. |
| `venueId` | `Long` | Identifier of the venue it belongs to. |

Returns a `PoiContent?` (the lightweight version), or `null` if not available or if the module isn't initialized.

!!! note
    The operation is only performed if `MultimediaFacade` has been correctly initialized. Use this method after synchronizing `PoiContent` items with `fetchPoiContentDetails(venueId, poiContentList)`.

### Get a detailed POI content
`getPoiContentDetailed(id, venueId)` retrieves the detailed version of a POI content associated with a venue.

| Parameter | Type | Description |
|-----------|------|--------------|
| `id` | `String` | Identifier of the POI content. |
| `venueId` | `Long` | Identifier of the venue it belongs to. |

Returns a `PoiContentDetail?` (the full detail), or `null` if not available or if the module isn't initialized.

!!! note
    The operation is only performed if `MultimediaFacade` has been correctly initialized. Use this method after synchronizing `PoiContent` items with `fetchPoiContentDetails(venueId, poiContentList)`.

### Get HTML categories
`getHtmlCategories(venueId)` retrieves the HTML categories available locally.

| Parameter | Type | Description |
|-----------|------|--------------|
| `venueId` | `Long?` | Identifier of the venue. |

Returns a `List<HtmlCategories>?`, or `null` if not available or if the module isn't initialized.

=== "Android"
    ```kotlin
    val categories = nextomeSdk.getHtmlCategories(134L)

    // or

    val categories = nextomeSdk.getHtmlCategories(null)
    ```

!!! note
    The operation is only performed if `MultimediaFacade` has been correctly initialized. If a `venueId` is specified, results include both system-level HTML categories and those registered for the given venue. If `venueId` is `null`, only system-level HTML categories are returned.

### Get tags
`getTags(venueId)` retrieves the Tags available locally.

| Parameter | Type | Description |
|-----------|------|--------------|
| `venueId` | `Long?` | Identifier of the venue. |

Returns a `List<Tag>?`, or `null` if not available or if the module isn't initialized.

=== "Android"
    ```kotlin
    val tags = nextomeSdk.getTags(134L)

    // or

    val tags = nextomeSdk.getTags(null)
    ```

!!! note
    The operation is only performed if `MultimediaFacade` has been correctly initialized. If a `venueId` is specified, results include both system-level Tags and those registered for the given venue. If `venueId` is `null`, only system-level Tags are returned.

## PoiContentPaginatorFacade

`PoiContentPaginatorFacade` is a component that manages pagination of poi-contents (*Point Of Interest Contents*) associated with a specific venue.

It encapsulates the progressive data loading logic, exposes state through `StateFlow`, and handles errors, allowing the UI layer to easily observe state changes.

Typical flow:

1. `initPaginator() -> loadNextPage() -> observe items -> loadNextPage() -> endReached = true`
2. `initPaginator() -> loadAllPages() -> wait until end -> endReached = true`

### Constructor

```kotlin
PoiContentPaginatorFacade(venueId: Long? = null)
```

| Parameter | Type | Description |
|-----------|------|--------------|
| `venueId` | `Long?` | Identifier of the venue to retrieve contents for. If `null`, the repository will use its own default behavior. |

### Observable properties

The following properties are exposed as `StateFlow` and can be observed from the UI layer.

#### isLoading
`StateFlow<Boolean>` — indicates whether a load request is currently in progress.

| Value | Meaning |
|-------|---------|
| `true` | A page is currently being loaded. |
| `false` | No loading operation in progress. |

=== "Android"
    ```kotlin
    lifecycleScope.launch {
        paginator.isLoading.collect { loading ->
            progressBar.isVisible = loading
        }
    }
    ```

!!! note
    See the iOS example in [Observe the paginator state](#observe-the-paginator-state).

#### items
`StateFlow<List<PoiContent>>` — contains the list of POI contents loaded so far.

=== "Android"
    ```kotlin
    lifecycleScope.launch {
        paginator.items.collect { items ->
            adapter.submitList(items)
        }
    }
    ```

!!! note
    See the iOS example in [Observe the paginator state](#observe-the-paginator-state).

#### error
`StateFlow<Throwable?>` — contains any error that occurred during loading.

=== "Android"
    ```kotlin
    lifecycleScope.launch {
        paginator.error.collect { throwable ->
            throwable?.let {
                showErrorDialog(it.message ?: "Unknown error")
            }
        }
    }
    ```

!!! note
    See the iOS example in [Observe the paginator state](#observe-the-paginator-state).

#### endReached
`StateFlow<Boolean>` — indicates whether all available pages have been loaded.

| Value | Meaning |
|-------|---------|
| `true` | No further pages to load. |
| `false` | More pages are available. |

=== "Android"
    ```kotlin
    lifecycleScope.launch {
        paginator.endReached.collect { endReached ->
            if (endReached) {
                footerView.isVisible = false
            }
        }
    }
    ```

!!! note
    See the iOS example in [Observe the paginator state](#observe-the-paginator-state).

### Methods

#### loadAllPages()
Loads all available pages until the last page is reached.

=== "Android"
    ```kotlin
    viewModelScope.launch {
        paginator.loadAllPages()
    }
    ```

!!! note
    Pages are loaded sequentially. The operation stops automatically once `endReached` becomes `true`.

#### loadNextPage()
Loads the next available page.

=== "Android"
    ```kotlin
    viewModelScope.launch {
        paginator.loadNextPage()
    }
    ```

    In a `LazyColumn` (Jetpack Compose):

    ```kotlin
    LaunchedEffect(lastVisibleItem) {
        if (lastVisibleItem >= items.size - 3) {
            paginator.loadNextPage()
        }
    }
    ```

!!! note
    Automatically updates `isLoading`, `items`, `error` and `endReached`.

#### reset()
Resets the paginator to its initial state.

=== "Android"
    ```kotlin
    fun refreshContent() {
        paginator.reset()

        viewModelScope.launch {
            paginator.loadNextPage()
        }
    }
    ```

!!! note
    Resets the pagination key, clears the loaded items, and clears the current error.
