---
uid: Meadow.Foundation.Sensors.Motion.Mag3110
remarks: *content
---

| Mag3110 | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Motion.Mag3110) |
| Datasheet(s) | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Motion.Mag3110/Datasheet) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.Sensors.Motion.Mag3110/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.Sensors.Motion.Mag3110.svg?label=Meadow.Foundation.Sensors.Motion.Mag3110" alt="NuGet Gallery for Mag3110" /></a> |

The **MAG3110** is a three axis magnetometer with an I2C interface. The magnetometer is capable of single and continuous readings.

### Code Example

```csharp
Mag3110 sensor;

public MeadowApp()
{
    Console.WriteLine("Initializing");

    sensor = new Mag3110(Device.CreateI2cBus());

    // classical .NET events can  be used
    sensor.Updated += (sender, result) => {
        Console.WriteLine($"Magnetic Field: [X:{result.New.MagneticField3D?.X.MicroTesla:N2}," +
            $"Y:{result.New.MagneticField3D?.Y.MicroTesla:N2}," +
            $"Z:{result.New.MagneticField3D?.Z.MicroTesla:N2} (MicroTeslas)]");

        Console.WriteLine($"Temp: {result.New.Temperature?.Celsius:N2}C");
    };

    // Example that uses an IObservable subscription to only be notified when the filter is satisfied
    var consumer = Mag3110.CreateObserver(
        handler: result => Console.WriteLine($"Observer: [x] changed by threshold; new [x]: X:{result.New.MagneticField3D?.X.MicroTesla:N2}, old: X:{result.Old?.MagneticField3D?.X.MicroTesla:N2}"),
        // only notify if there's a greater than 1 micro tesla on the Y axis
        filter: result => {
            if (result.Old is { } old) { //c# 8 pattern match syntax. checks for !null and assigns var.
                return ((result.New.MagneticField3D - old.MagneticField3D)?.Y > new MagneticField(1, MU.MicroTesla));
            }
            return false;
        });
    sensor.Subscribe(consumer);

    //==== one-off read
    ReadConditions().Wait();

    // start updating
    sensor.StartUpdating(TimeSpan.FromMilliseconds(500));
}

protected async Task ReadConditions()
{
    var result = await sensor.Read();
    Console.WriteLine("Initial Readings:");
    Console.WriteLine($"Mangetic field: [X:{result.MagneticField3D?.X.MicroTesla:N2}," +
        $"Y:{result.MagneticField3D?.Y.MicroTesla:N2}," +
        $"Z:{result.MagneticField3D?.Z.MicroTesla:N2} (microteslas)]");

    Console.WriteLine($"Temp: {result.Temperature?.Celsius:N2}C");
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Motion.Mag3110/Samples/Mag3110_Sample)

### Polling Mode

The following application reads the values from the magnetometer and displays the readings on the debug output:

```csharp
public class MeadowApp : App<F7Micro, MeadowApp>
{
    public MeadowApp()
    {
        Console.WriteLine("MAG3110 Test Application");
        Mag3110 mag3110 = new Mag3110();
        mag3110.Standby = false;
        int readingCount = 0;

        while (true)
        {
            mag3110.Read();
            readingCount++;
            Console.WriteLine(
                "Reading " + readingCount.ToString() + 
                ": x = " + mag3110.X.ToString() + 
                ", y = " + mag3110.Y.ToString() + 
                ", z = " + mag3110.Z.ToString());
            Thread.Sleep(1000);
        }
    }
}
```

### Wiring Example

In it's basic configuration the magnetometer requires four connections:

| Meadow Pin   | Sensor Pin     | Wire Color |
|--------------|----------------|------------|
| 3.3V         | V<sub>cc</sub> | Red        |
| GND          | GND            | Black      |
| SC           | SCK            | Green      |
| SD           | SDA            | Blue       |
| D8           | INT1           | Orange     |

<img src="../../API_Assets/Meadow.Foundation.Sensors.Motion.MAG3110/MAG3110_Fritzing.svg" 
    style="width: 60%; display: block; margin-left: auto; margin-right: auto;" />




