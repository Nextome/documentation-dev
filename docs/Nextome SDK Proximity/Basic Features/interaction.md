## Interaction

Interaction is the output of the Proximity SDK for the host application. 
It must be set from the portal in the appropriate section.

### NextomeInteraction

The `NextomeInteraction` class is described below. 

=== "Android"
| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `id: Int?`   | The id of the interaction   | null |
| `name: String?`  | The name of the interaction | null |
| `genericContent: String?`   | A generic content provided by our portal   | null |
| `contents: List<NextomeInteractionContent>?`| A list of contents provided by our portal | null | 
=== "iOS"
| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `id: Int?`   | The id of the interaction   | null |
| `name: String?`  | The name of the interaction | null |
| `genericContent: String?`   | A generic content provided by our portal   | null |
| `contents: List<NextomeInteractionContent>?`| A list of contents provided by our portal | null | 
-->

If you need to transport custom informations you can use just genericContent. 
If you need to specify informations for different languages you can use contents field.

### NextomeInteractionContent

The `NextomeInteractionContent` class is described below

=== "Android"
| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `id: Int?`   | The id of the interaction content   | null |
| `language: String?`  | The language | null |
| `content: String?`   | A specific content provided by our portal for specified language   | null |
| `image: String?`   | A specific image url provided by our portal for specified language    | null |
| `link: String?`   | A specific link provided by our portal for specified language    | null |
| `displayLink: String?`   | The text placeholder for provided link | null |
=== "iOS"
| Property             | Description                          | Default |
| :--------------------| :----------------------------------- | :-------
| `id: Int?`   | The id of the interaction content   | null |
| `language: String?`  | The language | null |
| `content: String?`   | A specific content provided by our portal for specified language   | null |
| `image: String?`   | A specific image url provided by our portal for specified language    | null |
| `link: String?`   | A specific link provided by our portal for specified language    | null |
| `displayLink: String?`   | The text placeholder for provided link | null |
-->

!!! note
    If you want to always show a specific indoor map and enable the localization only for a specific venue you can combine `Initial resources` and `Force specific venue` functionality.
    Use `getVenueResources` to retrieve the image resources to  initialize the map and use the callback from the `Getting started` section to react on position updates and floor changes.