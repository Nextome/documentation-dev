# Nextome SDK - iOS Changelog
!!!note
    If you want to user SDK to specify the version, then explicitly like this
    ```swift
        pod 'NextomeLocalization', '3.2.1'

    ```

### 3.2.1 | March 2026
 * BREAKING - Removed FindFloorState and replaced with EvaluateIndoorOutdoorState;
 * Better distinction between the various types of logs;
 * EXPERIMENTAL - Added turn-by-turn navigation. See documentation for more;
 * Minor bug fixes;

### 3.0.7 | December 2025
 * Added georeferenced and outdoor fields on map entity;
 * Added sending outdoor positions from outside;
 * Added intenrally check on permissions;

### 3.0.6 | October 2025
 * Added pre-resources download based on GPS position;
 * Added pre-resources download via venue specification;
 * Added support for weighted path and with 2 algoritm for path finder (a-start, dijkstra);
 * Check for resource download progress;
 * Replaced events with fences;
 * Path finder algorithm fix;
 * Exposed gpcs (Ground Control Points), rooms, pois; 

### 3.0.1
 * New SDK internal architecture;
 * bugfixes and improvements;

### 2.0.0-rc4

Add support for xCode 15

### 2.0.0-rc3

Bugfix and improvements

### 2.0.0-rc1

New version is here! 🎉
