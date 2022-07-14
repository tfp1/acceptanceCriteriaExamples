# Product Requirements

1. Ability to force {{this}} device to the 2.4 band for IoT onboarding
2. Information to explain the use of this feature
3. Ability to clear the 2.4-only profile early
4. Ability to know in the app the device is in a 2.4-only profile && to know remaining duration
5. A mechanism for identifying which device is going to be kicked so the user can confirm it’s the intended device
  *  This is because we can’t get IP Address from the native SDKs and our mechanism for identifying {{this}} device relies on IP address which is known to be unreliable in existing features

# Technical Requirements

1. Endpoints
```
PUT    /Customers/:customerId/locations/:locationId/devices/:macAddress/forcedSteer
DELETE /Customers/:customerId/locations/:locationId/devices/:macAddress/forcedSteer
GET    /Customers/:customerId/Locations/:locationId/Devices/:macAddress
GET    /Customers/:customerId/Locations/:locationId/Devices{noformat}
```

2. `GET /devices/:macAddress` returns a `steering`` object
```
"steering":{
  "forced":{
    "freqBand":"2.4G",
    "expiresAt":"2022-03-09T19:57:59.223Z"
  }
```
3. If the `device` does not have a profile set, we would see
```
"steering":{}
```
4. `GET /devices` returns the `steering` object for all devices on the location
5. Capabilities requirement: `Client Steering` is enabled
6. Remote Config
  * `iot_onboarding_enabled`
  * Local default: false
  * Remote default: false
  * Add a task to change the local default to TRUE after we’ve cleared QA
  * Update confluence documentation with new flag
7. Add a button to start this mode on `device profile`
  * Method to start iot onboarding mode
  * Indicator if `this` device is in iot onboarding mode
  * Remaining time for mode
  * Figma #1 with with `this` device info
  * Figma #2 in active state with no new devices in the network
  * Figma #3 with at least 1 new device connected to the network

Tested cases for API work defined in SOBI ticket 

h1. Open Questions

1. Where does the button to start this mode go since we don’t have Quick Actions?
2. ~How do we GET a device's current profile state and remaining time?~
  * Added documentation for `GET /device/:macAddress` and `GET /devices`
3. ~How do we know which IoT modal to go to?~
  * If the mode is not enabled for this device, go to the first screen (Figma 1)
  * If it is enabled, go to the `we are waiting...` screen (Figma 2)
4. ~How do we end IoT onboarding after we’ve dismissed the modal?~
  * Figma updated with updated design elements
