---
title: Introduction
hide:
  - footer
---
# NEXTOME MAPVIEW DOCUMENTATION

The Nextome MapView is the basic UI component used to show the map and the informations on a them.

The Nextome MapView component is build on top of [flutter_map](https://pub.dev/packages/flutter_map) plugin.

Basically it's works on a layers system and consist of three main components: `Tiles`, `Layers` and `Markers`.

## TILES

The `Tile` is the background component used to show the map. More then one tile can be used into MapView. They can be ordered by render importance and can be enbaled or disabled in accordance of your needs.

## LAYERS

The `Layer` is the logical component that grouping the markers to manage better you informations.  

Its usage can be more clear with this example:

- We want to represents and emergency evacuation route plan and we want to show three different informations on the map: emergency exit, fire extinghushers and the evacuation routes. We can organize this three element in three different layers and turn-on/off according to needs. We can populate the first layer with the markers representing the emergency exit, next we can populate de second layer with the markers representing the fire fire extinghushers and at the end we can populate the last layer with list of path markers representing the evacuation routes. 
The layers can be orderd by a zindex parameter setted at the moment of its declaration.
So the grouping allow us to acting just on one layer, for example if we want to se only the evacation routes layer, we will turn-off the other two layers. 

Therefore the layer is just a logical collector of markers used to manage a group of markers at once.


## MARKERS
The `Marker` is the component related to the information that you wanto to show on a specific position on the map, like a POI.

It consist of different types:

- `Marker`, a marker represented by an image and some other customizations
- `Path Marker`, a marker that represents a path with a list of points specified and some other customizations
- `Shape Marker`, a marker that represents a polygon with a list of point specified and some other customizations

## Android Resources
[Android Docs](./integration.md) | [Changelog](./Changelog/changelog-android.md) | [Example Code](./example.md)

## iOS Resources
[iOS Docs](./integration.md#ios-integraton) | [Changelog](./Changelog/changelog-ios.md) | [Example Code](./example.md)
