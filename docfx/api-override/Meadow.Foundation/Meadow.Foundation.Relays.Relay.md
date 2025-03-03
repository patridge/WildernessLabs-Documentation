---
uid: Meadow.Foundation.Relays.Relay
remarks: *content
---

| Relay | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Core/Relays) |
| Datasheet(s) | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Audio.Mp3.Yx5300/Datasheet) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.svg?label=Meadow.Foundation" alt="NuGet Gallery for Relay" /></a> |

A relay is an electrically operated or electromechanical switch composed of an electromagnet, an armature, a spring and a set of electrical contacts. The electromagnetic switch is operated by a small electric current that turns a larger current on or off by either releasing or retracting the armature contact, thereby cutting or completing the circuit. Relays are necessary when there must be electrical isolation between controlled and control circuits, or when multiple circuits need to be controlled by a single signal.

[Sample projects available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/master/Source/Meadow.Foundation.Core.Samples) 

### Code Example

```csharp
protected Relay relay;

public MeadowApp()
{
    Console.WriteLine("Initializing...");

    relay = new Relay(Device.CreateDigitalOutputPort(Device.Pins.D02));

    TestRelay();
}

protected void TestRelay()
{
    Console.WriteLine("TestRelay...");

    var state = false;

    while (true)
    {
        state = !state;

        Console.WriteLine($"- State: {state}");
        relay.IsOn = state;

        Thread.Sleep(500);
    }
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Core.Samples/Relays.Relay_Sample)

### Wiring Example

<img src="../../API_Assets/Meadow.Foundation.Relays.Relay/Relay_Fritzing.svg" 
