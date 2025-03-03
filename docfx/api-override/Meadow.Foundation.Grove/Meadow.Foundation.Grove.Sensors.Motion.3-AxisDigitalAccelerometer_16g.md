---
uid: Meadow.Foundation.Grove.Sensors.Motion.ThreeAxisDigitalAccelerometer16g
remarks: *content
---

| 3-AxisDigitalAccelerometer_16g | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation.Grove/tree/main/Source/3-AxisDigitalAccelerometer_16g) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.Grove.Sensors.Motion.3-AxisDigitalAccelerometer_16g/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.Grove.Sensors.Motion.3-AxisDigitalAccelerometer_16g.svg?label=Meadow.Foundation.Grove.Sensors.Motion.3-AxisDigitalAccelerometer_16g" alt="NuGet Gallery for 3-AxisDigitalAccelerometer_16g" /></a> |

### Code Example

```csharp
public MeadowApp()
{
    Console.WriteLine("Initializing");

    var sensor = new ThreeAxisDigitalAccelerometer16g(Device.CreateI2cBus());
    sensor.SetPowerState(false, false, true, false, ThreeAxisDigitalAccelerometer16g.Frequencies.TwoHz);

    // classical .NET events can also be used:
    sensor.Updated += (sender, result) =>
    {
        Console.WriteLine($"Accel: [X:{result.New.X.MetersPerSecondSquared:N2}," +
            $"Y:{result.New.Y.MetersPerSecondSquared:N2}," +
            $"Z:{result.New.Z.MetersPerSecondSquared:N2} (m/s^2)]");
    };

    // start updating
    sensor.StartUpdating(TimeSpan.FromMilliseconds(500));
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation.Grove/tree/main/Source/3-AxisDigitalAccelerometer_16g/Sample/3-AxisDigitalAccelerometer_16g_Sample)

### Wiring Example

| ThreeAxisDigitalAccelerometer16g | Meadow Pin |
|--------|------------|
| GND    | GND        |
| VCC    | 3.3V       |
| RX     | D01        |
| TX     | D00        |
