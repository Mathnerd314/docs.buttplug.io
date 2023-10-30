# Spec Changelog

## Version 4 (2023-07-04)

- Messages Changed:
  - DeviceList/DeviceAdded
    - Moved from message definitions to per-actuator/sensor definitions
    - Fixed issues with indexes of actuators
    - Fixed issues with indexes of sensors in relation to their capabilities (read/subscribe)

## Version 3 Patch 3 (2022-12-30)

- Message Definitions Fixed:
  - The `SensorType` and `SensorRange` message attributes are valid for SensorSubscribeCmd as well
    as SensorReadCmd.

## Version 3 Patch 2 (2022-12-27)

- Message Definitions Fixed:
  - DeviceAdded/DeviceList use `DeviceMessageTimingGap`, not `DeviceMessageGap`.
  - LinearCmd/RotateCmd information in the message attributes of DeviceAdded/DeviceList will have an
    ActuatorType (Previously stated that only ScalarCmd has an actuator type).

## Version 3 Patch 1 (2022-10-14)

- Message Descriptions Changed:
  - ServerInfo
    - The original idea behind ServerInfo's message spec version output was to notify clients if a
      new, higher version of the message spec might be available, prompting for upgrade.
      Unfortunately, a long running programming error in multiple official reference libraries will
      cause connections to fail if the server lists its maximum available spec version and that
      version is higher than the client's spec version. Therefore, ServerInfo cannot return a
      version that is higher than the client's spec verison. ServerInfo should return either a
      version that matches the clients, or throw an error if it cannot match the client's version.
      This does not change the structure of the message, just the expectations of how it should
      function.

## Version 3 (2022-08-29)

- Messages Added:
  - ScalarCmd
    - Replaces VibrateCmd and adds ability to easily extend with new actuator types that take a
      single value.
  - SensorReadCmd
    - Replaces Battery/RSSI messages and adds ability to easily extend with new sensor types.
  - SensorSubscribeCmd
    - Allows users to receive realtime updates from devices (pressure sensors kegelcizers,
      accelerometers in toys that have them, etc...)
  - SensorUnsubscribeCmd
  - SensorReading
    - Data returned from either sensor being read, or a subscription event.
- Messages Changed:
  - DeviceList/DeviceAdded
    - Remove _FeatureCount_, Message Attributes are now an array of attribute objects instead of
      many fields of arrays that had to be reconstructed. Should reduce bookkeeping.
    - Added Message Attributes _FeatureDescriptor_, _ActuatorType_, _SensorType_
    - Added Device Attributes _DisplayName_, _DeviceMessageGap_
    - For messages that have matching "undo" types, like RawSubscribe/RawUnsubscribe or
      SensorSubscribe/SensorUnsubscribe, only the initial command is relayed in the message attributes of _DeviceAdded_ or _DeviceList_. The arguments for these commands are the same, and it's assumed that if you can do something that has a matching undo, you'll only need to know about one.
- Messages Deprecated:
  - VibrateCmd
    - Superceded by ScalarCmd. Will still be available via API calls in client APIs, just no longer
      needs to be a specific message in the protocol.
  - BatteryLevelCmd
    - Superceded by SensorReadCmd
  - RSSILevelCmd
    - Superceded by SensorReadCmd
  - BatteryLevelReading
    - Superceded by SensorReading
  - RSSILevelReading
    - Superceded by SensorReading

## Version 2 (2020-09-28)

- Messages Added:
  - RawWriteCmd
  - RawReadCmd
  - RawReading
  - RawSubscribeCmd
  - RawUnsubscribeCmd
  - BatteryLevelCmd
  - BatteryLevelReading
  - RSSILevelCmd
  - RSSILevelReading
- Messages Changed:
  - DeviceList/DeviceAdded
    - Adding StepCount to Message Attributes, to let users know how
      many steps a feature can use (i.e. how many vibration levels a
      piece of hardware might have)
  - ServerInfo
    - Remove Version fields
- Messages Deprecated:
  - LovenseCmd
    - Superceded by VibrateCmd/RotateCmd/Raw\*Cmd. The protocol messages were originally meant to
      map generic -> protocol -> raw, but the protocols change quickly enough that it's not worth it
      to encode that at the protocol level. From v2 of the spec on, we will try to encode as many
      actions as possible in generic messages. For anything we haven't mapped yet, Raw\*Cmd can be
      used, though it's not a great idea due to security concerns.
    - LovenseCmd was never implemented in any of the Buttplug reference libraries, so removal
      shouldn't affect anything.
  - KiirooCmd
    - Superceded by VibrateCmd/LinearCmd/Raw*Cmd. See above for more explanation.
    - Only implemented by the Kiiroo Pearl 1 and Onyx 1 in Buttplug C#. Not sure it was ever used
      anywhere.
  - VorzeA10CycloneCmd
    - Superceded by RotateCmd/PatternCmd. See above for more explanation.
    - Implemented for the Vorze A10 Cyclone in C# and JS, but translates directly to rotation
      messages.
  - FleshlightLaunchFW12Cmd
    - Superceded by LinearCmd/Raw\*Cmd. See LovenseCmd reason for more explanation.
    - Implemented for the Fleshlight Launch, and will be problematic to switch out. We should still
      support it on the server side for v0/v1 for compat.
  - Test
    - Violates assumptions that client/server sends different message types. Also, not particularly
      useful.
  - RequestLog/Log
    - Allows too much information leakage across the protocol in situations we may not want, and
      also has nothing to do with sex toy control. Logging is an application level function, not
      really required in the protocol itself.

## Version 1 (2017-12-11)

- Messages Added:
  - VibrateCmd
  - LinearCmd
  - RotateCmd
- Messages Changed:
  - DeviceList/DeviceAdded
    - Added Message Attributes blocks to device info, with FeatureCount attribute
  - RequestServerInfo
    - Added Spec Version Field
- Messages Deprecated:
  - SingleMotorVibrateCmd
    - Superceded by VibrateCmd

## Version 0 (2017-08-24)

- First version of spec
- Messages Added:
  - Ok
  - Error
  - Log
  - RequestLog
  - Ping
  - Test
  - RequestServerInfo
  - ServerInfo
  - RequestDeviceList
  - DeviceList
  - DeviceAdded
  - DeviceRemoved
  - StartScanning
  - StopScanning
  - ScanningFinished
  - SingleMotorVibrateCmd
  - FleshlightLaunchFW12Cmd
  - LovenseCmd
  - KiirooCmd
  - VorzeA10CycloneCmd
  - StopDeviceCmd
  - StopAllDevices
