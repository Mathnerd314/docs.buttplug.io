# Actuators

## Acuator System Design

Buttplug provides 3 "levels" of device actuator control:

### Scalar (or 1D) General Control

Summary: _Send a number, make a thing happen_

This refers to commands that take a single value, which is between 0.0-1.0 (usually represented at a
64-bit float, of which we'll normally use a tiny fraction). What that command actually does to make
a toy move is ignored, and it is assumed that 0.0 is _off/stopped_ and 1.0 is _max
power/intensity/etc_. Whether that assumption is correct is unknown, since the action context has been dropped.

Buttplug Client Implementations written by the Buttplug Core Team expose these as `Scalar` actuators.

General scalar control is handy if you want to quickly make sure your code works with whatever hardware you have. More importantly, it's perfect if you just want to crank out an application that controls a sex toy so you can go _"hey look I made this thing control a sex toy"_ on social media and then be done with it.

:::warning The Fucking Machine Problem

The simplicity of ignoring device context comes with a cost, which we refer to as **The Fucking Machine Problem**. Vibrators, which are what most people use sex toy applications with, take a speed. Most affordable fucking machines also take a speed value, with which they drive a motor to make their thrusting actuator go back and forth. These two motions are wildly different. One might make the user numb, the other might perforate the the user. Using general control drops this context, which means the onus of safety is on the user. While there is little to nothing a developer can do to limit a user's choice of hardware and application pairings, keep this problem in mind when communicating about applications that use this type of control.

:::

### Scalar Control with Context

Summary: _Send a number with a specific context, make a thing of that context happen_

Scalar Control with Context is a refinement to add context to the number sent over. The 0.0-1.0
range is still there, but now the developer also pays attention to the different actuator types.
These types represent outputs like vibration speed, oscillation speed, etc... So instead of just
sending over a number and saying "make the device do a thing", the developer can specifically look
for devices that perform the output they're interested in.

Buttplug Client Implementations written by the Buttplug Core Team expose these as contextual
actuators, like `Vibrate`, `Rotate`, `Oscillate`, etc...

This is nice for either creating context specific actions and effects if a user has capable hardware. As mentioned in the prior section, we know that a _vibrate_ actuator will react different to commands than a _oscillate_ actuator, and a _position_ actuator make not even make sense in the assumed "higher value = more intensity" situation of using scalar values with no context.

It also gives the user more information when presented with a binding interface, for instance, if a game allows users to bind hardware interaction to certain parts of/actions on an avatar. 

### Multidimensional Control with Context

Summary: _Send a set of data to craft what happens in a certain context_

Finally, there are specific contexts that require more information than just a single number. Some
devices can move to different positions over a set amount of time, or can take directions in which
to move along with a speed. While these devices can be driven using the first two levels of
commands, using them to the extent of their functions requires more information, which can be
provided with these commands.

Buttplug Client Implementations written byt the Buttplug Core Team expose these as compound
contextual actuators, like `RotateWithDirection`, `PositionWithDuration`, etc...

These types of systems are useful when hardware can do more than just run at a speed or be set to some level. For instance, when building things like Video Sync or games with synchronized interaction, the ability to set movement over a duration is required. 

While most professional control systems would use some sort of closed feedback loop system for that
kind of control (which could be done in Buttplug via a combination of commands, but maybe 1-2 pieces
of the 300+ pieces of hardware the library supports could use it), sex toys are usually made by the
lowest bidder, so this is the best we get most of the time.

Multidimensional control is usually reducable to 1D. A actuator that takes position/duration movement commands can also be an oscillate actuator, as long as the oscillation command timing is handled out of view of the developer. Similarly, a device with direction and rotation can be a simple rotation device that only exposes one direction through a scalar command.

:::info But how did things used to be?

For any of nerds out there that are curious about how Buttplug worked before this, continue reading this callout. Otherwise, **feel free to skip this**, as this section just provides background on how we got to the current design.

:::

## Actuator Categories

Now that we've got the design basis laid out, we can move on to the different types of actuation available for devices.

### Vibrate

### Oscillate

### Rotate

### Constrict

### Inflate

### Position

### RotationWithDirection

### PositionWithDuration

### Unknown