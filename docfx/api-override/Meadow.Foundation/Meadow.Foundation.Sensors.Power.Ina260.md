---
uid: Meadow.Foundation.Sensors.Power.Ina260
remarks: *content
---

| Ina260 | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Power.Ina260) |
| Datasheet(s) | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Power.Ina260/Datasheet) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.Sensors.Power.Ina260/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.Sensors.Power.Ina260.svg?label=Meadow.Foundation.Sensors.Power.Ina260" alt="NuGet Gallery for Ina260" /></a> |

### Code Example

```csharp
public MeadowApp()
{
    Console.WriteLine("Initialize...");
    var bus = Device.CreateI2cBus();
    var ina = new Ina260(bus);

    Console.WriteLine($"-- INA260 Sample App ---");
    Console.WriteLine($"Manufacturer: {ina.ManufacturerID}");
    Console.WriteLine($"Die: {ina.DieID}");
    ina.Updated += (s, v) =>
    {
        Console.WriteLine($"{v.New.Item2}V @ {v.New.Item3}A");
    };

    ina.StartUpdating(TimeSpan.FromSeconds(2));
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Power.Ina260/Samples/Ina260_Sample)

