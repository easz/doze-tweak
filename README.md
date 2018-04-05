# doze-tweak

Android Doze Tweaks

## Prerequisite 

 - Android 6+ with [Doze](https://developer.android.com/training/monitoring-device-state/doze-standby.html)
 - [adb](https://developer.android.com/studio/command-line/adb.html)

## Goal

 - understand Doze behaviour from its internal implementation
 - tune Doze parameters to further improve battery usage
 - without installing extra Apps
 - without root access

## Caveat

 - Original Doze (or Deep Doze) availble on Android 6+ and Light Doze on Android 7+.
 - Deep Doze would only work if there is any motion sensor available (fallback on ```Sensor.TYPE_SIGNIFICANT_MOTION```)
   - Some battery saving Apps would disable (i.e. actually ```restrict```) sensors to other Apps, but Deep Doze will still have to depend on motion sensors.  
 
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
```
Inspect current customized Doze settings. It returns ```null``` if no customized settings has been set.
```
$ adb shell settings get global device_idle_constants
null
```
Set customized Doze settings.
```
adb shell settings put global device_idle_constants light_after_inactive_to=15000,...
```
Reset customized Doze settings to default.
```
adb shell settings delete global device_idle_constants
```

## Insight

![Light Doze](diagram/light-doze.svg)

![Deep Doze](diagram/deep-doze.svg)
