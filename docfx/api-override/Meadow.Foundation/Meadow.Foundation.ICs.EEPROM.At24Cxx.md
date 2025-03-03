---
uid: Meadow.Foundation.ICs.EEPROM.At24Cxx
remarks: *content
---

| At24Cxx | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/ICs.EEPROM.At24Cxx) |
| Datasheet(s) | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/ICs.EEPROM.At24Cxx/Datasheet) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.ICs.EEPROM.At24Cxx/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.ICs.EEPROM.At24Cxx.svg?label=Meadow.Foundation.ICs.EEPROM.At24Cxx" alt="NuGet Gallery for At24Cxx" /></a> |

The **AT24Cxx** series of chips provide a mechanism for storing data that will survive a power outage or battery failure.  These EEPROMs are available in varying sizes and are accessible using the I2C interface.

### Purchasing

* [DS3231 with integrated EEPROM](https://www.amazon.com/s/ref=nb_sb_noss?url=search-alias%3Daps&field-keywords=ds3231)

### Code Example

```csharp
public MeadowApp()
{
    Console.WriteLine("Initialize hardware...");

    //256kbit = 256*1024 bits = 262144 bits = 262144 / 8 bytes = 32768 bytes
    //if you're using the ZS-042 board, it has an AT24C32 and uses the default value of 8192
    var eeprom = new At24Cxx(i2cBus: Device.CreateI2cBus(), memorySize: 32768);

    Console.WriteLine("Write to eeprom");
    eeprom.Write(0, new byte[] { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 });

    Console.WriteLine("Read from eeprom");
    var memory = eeprom.Read(0, 16);

    for (ushort index = 0; index < 16; index++)
    {
        Thread.Sleep(50);
        Console.WriteLine("Byte: " + index + ", Value: " + memory[index]);
    }

    eeprom.Write(3, new byte[] { 10 });
    eeprom.Write(7, new byte[] { 1, 2, 3, 4 });
    memory = eeprom.Read(0, 16);

    for (ushort index = 0; index < 16; index++)
    {
        Thread.Sleep(50);
        Console.WriteLine("Byte: " + index + ", Value: " + memory[index]);
    }
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/ICs.EEPROM.At24Cxx/Samples/At24Cxx_Sample)

### Wiring Example

To wire a At24Cxx to your Meadow board, connect the following:

| At24Cxx | Meadow Pin  |
|---------|-------------|
| GND     | GND         |
| SCL     | D08 (SCL)   |
| SDA     | D07 (SDA)   |
| VCC     | 3V3         |

It should look like the following diagram:

<img src="../../API_Assets/Meadow.Foundation.ICs.EEPROM.AT24Cxx/AT24Cxx_Fritzing.png" 
    style="width: 60%; display: block; margin-left: auto; margin-right: auto;" />




