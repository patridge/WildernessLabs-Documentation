---
layout: Meadow
title: Meadow Application Lifecycle Events
subtitle: Responding to application lifecycle events as the Meadow board triggers them.
---

Meadow connect several events that indicate important events in the process of launching and running your application, such as initialization, errors, and sleep states.

These events are available through methods on the `App` base class. You can overload these methods to have your own code executed when these lifecycle events happen.

## `Initialize`

This event is where you can set up the components your app will use. For example, you can declare an LCD display as a class-level field and then set it in the `Initialize` method when Meadow is ready, making it available for use later in your app's lifecycle.

In a simple example, you could do the same with the onboard LED.

```csharp
RgbPwmLed onboardLed;
...

public override Task Initialize()
{
    onboardLed = new RgbPwmLed(device: Device,
        redPwmPin: Device.Pins.OnboardLedRed,
        greenPwmPin: Device.Pins.OnboardLedGreen,
        bluePwmPin: Device.Pins.OnboardLedBlue,
        CommonType.CommonAnode);

    ...

    return base.Initialize();
}
```

## `Run`

After Meadow is initialized, your `Run` method is called. This is where you likely put most of your application's core logic.

For example, if you initialize an `RgbPwmLed` as shown in the `Initialize` example above, you could then pulse it repeatedly in your `Run` method like this.

```csharp
RgbPwmLed onboardLed;
...

public override async Task Run()
{
    while (true)
    {
        onboardLed.StartPulse(Color.Red, TimeSpan.FromMilliseconds(500));
        Thread.Sleep(TimeSpan.FromMilliseconds(500));
        onboardLed.Stop();
    }
}
```

## `OnShutdown`

Some Meadow apps run indefinitely, continually handling inputs and responding accordingly. When your Meadow application has a defined lifecycle, though, Meadow will trigger the `OnShutdown` method after your app has finished running. This gives you a chance to run any final operations or clean up before things are completed.

```csharp
public override void OnShutdown()
{
    // Make sure you app is ready to finish running.
}
```

## `OnError`

* `void OnError(Exception e, out bool recovered)`
```csharp
public override void OnShutdown()
{
    // Make sure you app is ready to finish running.
}
```

## `OnResume`

```csharp
public override void OnResume()
{
    // 
}
```

## `OnSleep`

```csharp
public override void OnSleep()
{
    // 
}
```

## `OnRecovery`

```csharp
public override void OnRecovery(Exception e)
{
    // 
}
```

## `OnUpdate`

```csharp
public override void OnUpdate(Version newVersion, out bool approveUpdate)
{
    // 
}
```

## `OnUpdateComplete`

```csharp
public override void OnUpdateComplete(Version oldVersion, out bool rollbackUpdate)
{
    // 
}
```

## `OnReset`

```csharp
public override void OnReset()
{
    // 
}
```

<!-- ## [Next - Input/Output](/Meadow/Meadow_Basics/IO/) -->
