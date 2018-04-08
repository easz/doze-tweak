# doze-tweak

Understand **device idle state** trainsition and some ```device_idle_constants``` settings examples
## Prerequisite 

 - Android 6+ with [Doze](https://developer.android.com/training/monitoring-device-state/doze-standby.html)
 - [adb](https://developer.android.com/studio/command-line/adb.html)

## Goal

 - understand Doze internal [implementation](https://github.com/aosp-mirror/platform_frameworks_base/blob/nougat-release/services/core/java/com/android/server/DeviceIdleController.java)
 - tune ```device_idle_constants``` settings to use Doze in a flexibel way
 - without installing extra Apps
 - without root access

## Caveat

 - Original Doze (or Deep Doze) available on Android 6+ and Light Doze only on Android 7+.
 - Deep Doze would only work if there is any motion sensor available (fallback on [significant motion sensor](https://github.com/aosp-mirror/platform_frameworks_base/blob/nougat-release/services/core/java/com/android/server/DeviceIdleController.java#L1379))
   - Some battery saving Apps would disable (i.e. actually [restrict](https://android.googlesource.com/platform/frameworks/native/+/nougat-release/services/sensorservice/SensorService.h#119)) sensors from other Apps, but Deep Doze will still have to [depend](https://github.com/aosp-mirror/platform_frameworks_base/blob/nougat-release/services/core/java/com/android/server/DeviceIdleController.java#L2248) on motion sensors.
 - tuning Doze parameters from external ```adb``` interface is not very convenient in case that you want to change them frequently and directly on your Android.
 
## Quick Start

Inspect current Doze parameters from ```deviceidle```
```
$ adb shell dumpsys deviceidle
  Settings:
    light_after_inactive_to=+5m0s0ms
    light_pre_idle_to=+10m0s0ms
    light_idle_to=+5m0s0ms
    light_idle_factor=2.0
    light_max_idle_to=+15m0s0ms
    light_idle_maintenance_min_budget=+1m0s0ms
    light_idle_maintenance_max_budget=+5m0s0ms
    min_light_maintenance_time=+5s0ms
    min_deep_maintenance_time=+30s0ms
    inactive_to=+30m0s0ms
    sensing_to=+4m0s0ms
    locating_to=+30s0ms
    ...
```
Inspect any customized Doze parameter. It returns ```null``` if none has been set.
```
$ adb shell settings get global device_idle_constants
null
```
Set customized Doze parameters.
```
adb shell settings put global device_idle_constants light_after_inactive_to=15000,...
```
@TODO: make some real examples to use!

Reset customized Doze parameters to default.
```
adb shell settings delete global device_idle_constants
```

## Doze Device Idle State Transition

![Light Doze](diagram/light-doze.svg)

![Deep Doze](diagram/deep-doze.svg)
