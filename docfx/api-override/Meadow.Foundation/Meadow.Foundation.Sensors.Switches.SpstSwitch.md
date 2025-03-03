---
uid: Meadow.Foundation.Sensors.Switches.SpstSwitch
remarks: *content
---

| SpstSwitch | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Core/Sensors/Switches) |
| Datasheet(s) | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Audio.Mp3.Yx5300/Datasheet) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.svg?label=Meadow.Foundation" alt="NuGet Gallery for SpstSwitch" /></a> |

**SpstSwitch** represents a simple, on/off, Single-Pole-Single-Throw (SPST) switch that closes a circuit to either ground/common or high:

<img src="../../API_Assets/Meadow.Foundation.Sensors.Switches.SpstSwitch/SPST_Switch.jpg" 
    style="width: 60%; display: block; margin-left: auto; margin-right: auto;" />

Use the [`CircuitTerminationType`](/API/CircuitTerminationType) to specify whether the other side of the switch terminates to ground or high.

The following example shows how to use a SPST switch:

```csharp
public class MeadowApp : App<F7Micro, MeadowApp>
{
    DigitalOutputPort _blueLED;
    SpstSwitch _spstSwitch;

    public MeadowApp()
    {
        _blueLED = new DigitalOutputPort(Device.Pins.OnboardLEDBlue, true);

        _spstSwitch = new SpstSwitch(Device.Pins.D13, CircuitTerminationType.High);
        _spstSwitch.Changed += (s, e) =>
        {
            Console.WriteLine("Switch Changed");
            Console.WriteLine("Switch on: " + _spstSwitch.IsOn.ToString());
        };

        Console.WriteLine("Initial switch state, isOn: " + _spstSwitch.IsOn.ToString());
    }
}
```

[Sample projects available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/master/Source/Meadow.Foundation.Core.Samples) 

### Code Example

```csharp
protected SpstSwitch spstSwitch;

public MeadowApp()
{
    Console.WriteLine("Initializing...");

    spstSwitch = new SpstSwitch(Device.CreateDigitalInputPort(Device.Pins.D02, InterruptMode.EdgeFalling, ResistorMode.InternalPullDown));
    spstSwitch.Changed += (s,e) => 
    {
        Console.WriteLine("Switch Changed");
        Console.WriteLine($"Switch on: {spstSwitch.IsOn}");
    };

    Console.WriteLine("SpstSwitch ready...");
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Core.Samples/Sensors.Switches.SpstSwitch_Sample)

### Wiring Example

<img src="../../API_Assets/Meadow.Foundation.Sensors.Switches.SpstSwitch/SpstSwitch_Fritzing.svg" 
