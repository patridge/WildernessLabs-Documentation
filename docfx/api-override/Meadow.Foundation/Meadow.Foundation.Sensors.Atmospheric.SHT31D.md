---
uid: Meadow.Foundation.Sensors.Atmospheric.Sht31D
remarks: *content
---

| Sht31d | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Atmospheric.Sht31d) |
| Datasheet(s) | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Atmospheric.Sht31d/Datasheet) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.Sensors.Atmospheric.Sht31D/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.Sensors.Atmospheric.Sht31D.svg?label=Meadow.Foundation.Sensors.Atmospheric.Sht31D" alt="NuGet Gallery for Sht31d" /></a> |

The **SHT31D** is a temperature and humidity sensor with a built in I2C interface. The sensor has a typical accuracy of +/- 2% relative humidity and +/- 0.3C.

### Code Example

```csharp
Sht31d sensor;

public MeadowApp()
{
    Console.WriteLine("Initializing...");

    sensor = new Sht31d(Device.CreateI2cBus());

    var consumer = Sht31d.CreateObserver(
        handler: result => 
        {
            Console.WriteLine($"Observer: Temp changed by threshold; new temp: {result.New.Temperature?.Celsius:N2}C, old: {result.Old?.Temperature?.Celsius:N2}C");
        },                
        filter: result => 
        {
            //c# 8 pattern match syntax. checks for !null and assigns var.
            if (result.Old is { } old) 
            { 
                return (
                (result.New.Temperature.Value - old.Temperature.Value).Abs().Celsius > 0.5
                &&
                (result.New.Humidity.Value.Percent - old.Humidity.Value.Percent) > 0.05
                ); 
            }
            return false;
        }
    );
    sensor.Subscribe(consumer);

    sensor.Updated += (sender, result) => 
    {
        Console.WriteLine($"  Temperature: {result.New.Temperature?.Celsius:N2}C");
        Console.WriteLine($"  Relative Humidity: {result.New.Humidity:N2}%");
    };
    
    ReadConditions().Wait();

    sensor.StartUpdating(TimeSpan.FromSeconds(1));
}

async Task ReadConditions()
{
    var conditions = await sensor.Read();
    Console.WriteLine("Initial Readings:");
    Console.WriteLine($"  Temperature: {conditions.Temperature?.Celsius:N2}C");
    Console.WriteLine($"  Relative Humidity: {conditions.Humidity?.Percent:N2}%");
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Sensors.Atmospheric.Sht31d/Samples/Sht31d_Sample)

#### Interrupt Mode

The application below generates and interrupt when the temperature or humidity changes by more than 0.1 &deg;C.  The sensor is checked every 100 milliseconds.

```csharp
public class MeadowApp : App<F7Micro, MeadowApp>
{
    public MeadowApp()
    {
        // Create a new SHT31D object that will generate interrupts when
        // the temperature changes by more than +/- 0.1C or the humidity
        // changes by more than 1%.
        SHT31D sht31d = new SHT31D(temperatureChangeNotificationThreshold: 0.1F,
            humidityChangeNotificationThreshold: 1.0F);

        // Hook up the two interrupt handlers to display the changes in
        // temperature and humidity.
        sht31d.HumidityChanged += (s, e) =>
        {
            Console.WriteLine("Current humidity: " + e.CurrentValue.ToString("f2"));
        };

        sht31d.TemperatureChanged += (s, e) =>
        {
            Console.WriteLine("Current temperature: " + e.CurrentValue.ToString("f2"));
        };

        // Main program loop can now go to sleep as the work
        // is being performed by the interrupt handlers.
        Thread.Sleep(Timeout.Infinite);
    }
}
```

#### Polling Mode

The application below polls the sensor every 1000 milliseconds and displays the temperature and humidity on the debug console:

```csharp
public class MeadowApp : App<F7Micro, MeadowApp>
{
    public MeadowApp()
    {
        SHT31D sht31d = new SHT31D(updateInterval: 0);

        Console.WriteLine("SHT31D Temperature / Humidity Test");

        while (true)
        {
            sht31d.Update();
            Console.WriteLine("Temperature: " + sht31d.Temperature.ToString("f2") + ", Humidity: " + sht31d.Humidity.ToString("f2"));
            Thread.Sleep(1000);
        }
    }
}
```

### Wiring Example

The SHT31D breakout board from Adafruit is supplied with pull-up resistors installed on the `SCL` and `SDA` lines.

The `ADR` line is tied low giving and I2C address of 0x44.  This address line can also be tied high and in this case the I2C address is 0x45.

<img src="../../API_Assets/Meadow.Foundation.Sensors.Atmospheric.SHT31D/SHT31D_Fritzing.svg" 
    style="width: 60%; display: block; margin-left: auto; margin-right: auto;" />




