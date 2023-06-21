# Devices and Commands

In this section, we'll be covering how device command and control works. While this was mentioned briefly in the [Your First Buttplug App section](/docs/dev-guide/writing-buttplug-applications/intro), here we'll be going into the specifics of the components devices are made up of, and how to send commands and receive data from different device components.

## What is a Device in Buttplug?

Devices in Buttplug are comprised of 3 different types of components.

- Actuators
  - This is just a fancy name for "Device Outputs". Vibration, rotation, moving back and forth, all
    of these are types of actuation. Buttplug devices expose a core set of actuator types that we expect all devices to conform to.
- Sensors
  - While actuators are all about output, sensors are how we get data back from a device. This can
    be benign status information like battery levels, or something that relates to the usage of a toy, like a pressure sensor or accelerometer.
- Raw Endpoints
  - Raw Endpoints are a special component that should only be used in very specific development
    circumstances (and must be explicitly turned on in the Buttplug Server to use). Raw allows
    Buttplug to act as a simple passthru layer for hardware. Instead of sending specific actuator or
    sensor commands, raw allows byte arrays to be sent to/received from a device. 

We'll be discussing actuators and sensors here, while raw endpoints are discusses only in the Implementing New Devices and Clients section of the developer guide.

## Device Design

Before we get into the specifics of actuators, let's cover device command design in Buttplug.

At its core, Buttplug is made to give developers access to the widest variety of devices possible,
with a balance to rapid development capability and granuality. As we're never quite sure what people
will use the library for or what devices they'll do with it, this is an incredibly challenging
situation to deal with.

Buttplug provides developers with a top-down approach to device control. First and foremost, developers should be able to quickly and easily write code that gets any device available moving with events from their program. After that, where they stop in terms of control contexts and granularity is up to them.

For developers just making a quick game mod shitpost, they may use the most general commands possible to get things out the door. For game devs making haptics an integral part of the player experience, they may provide access to user-controlled device mappings for complex, possibly self-built hardware. 

We're definitely not the first technology to deal with the problem of general device access, though we may be the first to do so specifically with intimate haptics. Other projects have achieved various levels of success trying this with different types of hardware. Where Buttplug has ended up is a combination of learning from other projects and listening to developers as they've used prior versions of the system.

:::info Where did this design come from?

To build what we've got, lessons were taken from other generic hardware access projects and specification. A few of these are listed in the collapsed details section below for those interested.

<details>

### Example: The USB HID Spec

The USB Implementors Forum maintains the USB Human Interface Device Specification, which is supposed to be a one stop shop for all types of interface devices for computers. Mice, Keyboards, media function buttons, and other hardware you interact with daily are all covered under this specification.

However, many devices you may never knew existed are also supported! Golf Clubs! Barcode Scanners! Magic Carpets (NOT REALLY)! The list goes on and on and on, and you can read all 430+ pages of it in the [USB HID Usage Tables Document](https://www.usb.org/document-library/hid-usage-tables-14).

Obviously, this amount of device support is untenable for implementation by a small open source
team. Trying to specify that many devices means creates a system so vast that the protocol must be extremely general, and the developer must do quite a bit of work to figure out what capabilities the device they're seeing has, what are the features of those capabilities, etc... While it's certainly a task that needs to happen for projects the scale of an operating system, we can scope smaller than this.

### Example: VRPN and H3D

Scaling down some, there's projects like VRPN, the Virtual Reality Peripherals Network. VRPN has been around since well before the current VR era, supporting hardware all the way back to the days of serial port input controllers and CAVE systems.

Similarly, there's haptics engines like H3D, that allow developers to define generic workspaces and materials for communicating haptics experiences.

The interesting part about these solutions is that they can define usage areas (i.e. we know a person needs to have a headset, have a vague idea of what inputs might be, as well as sensors for positions for VR, or will be using some sort of touch feedback for ), but still allows

</details>

:::

## Sensors


