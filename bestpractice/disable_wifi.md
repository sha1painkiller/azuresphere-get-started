# How to disable Wi-Fi on MT3620

1. HEADER -> #include <applibs/networking.h>
1. Declare network capability in app_manifest.json
- `"NetworkConfig" : true`
2. Enable/Disable a network interface
- `int Networking_SetInterfaceState(const char *networkInterfaceName, bool isEnabled);`
- for ex: `Networking_SetInterfaceState("wlan0", False);`
