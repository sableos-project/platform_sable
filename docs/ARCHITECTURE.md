# Common Sable platform architecture

The platform layer sits between Sable applications and substrate/device integration.

```text
Sable applications / shell
        |
        | stable semantic APIs
        v
common Sable contracts and services
        |
        | bounded Android adapters
        v
Android framework / supported HAL interfaces
        |
        v
device/vendor implementation
```

## Design goals

- keep semantic APIs stable across Android releases and devices;
- keep framework hooks small and reviewable;
- preserve Android security boundaries such as Binder, SELinux, permissions, AppOps, PackageManager, and supported HAL interfaces;
- avoid exposing raw device/vendor authority to presentation code;
- use stable AIDL/C/NDK boundaries where independent toolchains must interoperate;
- keep device-specific compatibility work out of common semantic code.

## Service pattern

Preferred pattern:

```text
UI/app -> typed semantic request -> narrow Sable service -> Android framework/HAL
```

Avoid turning the platform layer into a generic privileged command executor.

## Initial portability target

The same common contracts should be consumable by Panther/Android 17 and future Bramble/Android 16 experiments without redesigning user-facing semantics.
