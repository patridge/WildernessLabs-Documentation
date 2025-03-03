---
uid: Meadow.Foundation.Displays.Lcd.CharacterDisplay
remarks: *content
---

| CharacterDisplay | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Displays.Lcd.CharacterDisplay) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.Displays.Lcd.CharacterDisplay/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.Displays.Lcd.CharacterDisplay.svg?label=Meadow.Foundation.Displays.Lcd.CharacterDisplay" alt="NuGet Gallery for CharacterDisplay" /></a> |


The **CharacterDisplay** class represents a multiline liquid crystal character display.

These displays are commonly found in 16x2 and 20x4 character by row configurations. Each character is represented using a grid of 5x7 pixels.

The current driver implementation uses 4 or 8 GPIO pins. It's common to find controller boards with this displays that provide I2C, SPI, or serial control. These will be supported in a future update.

### Code Example

```csharp
CharacterDisplay display;

public MeadowApp()
{
    //InitGpio();
    //InitGpioWithPWM();
    //InitI2c();
    InitGrove();

    TestCharacterDisplay();

    Console.WriteLine("Test complete");
}

void InitGpio() 
{
    Console.WriteLine("InitGpio...");
    
    display = new CharacterDisplay
    (
        device: Device,
        pinRS: Device.Pins.D10,
        pinE: Device.Pins.D09,
        pinD4: Device.Pins.D08,
        pinD5: Device.Pins.D07,
        pinD6: Device.Pins.D06,
        pinD7: Device.Pins.D05,
        rows: 4, columns: 20
    );
}

void InitGpioWithPWM()
{
    Console.WriteLine("InitGpioWithPWM...");

    display = new CharacterDisplay
    (
        device: Device,
        pinV0: Device.Pins.D11,
        pinRS: Device.Pins.D10,
        pinE:  Device.Pins.D09,
        pinD4: Device.Pins.D08,
        pinD5: Device.Pins.D07,
        pinD6: Device.Pins.D06,
        pinD7: Device.Pins.D05,
        rows: 4, columns: 20
    );
}

void InitI2c()
{
    Console.WriteLine("InitI2c...");

    display = new CharacterDisplay
    (
        i2cBus: Device.CreateI2cBus(I2cBusSpeed.Standard),
        address: (byte)I2cCharacterDisplay.Addresses.Default,
        rows: 4, columns: 20
    );
}

void InitGrove()
{
    Console.WriteLine("InitGrove...");

    display = new CharacterDisplay
    (
        i2cBus: Device.CreateI2cBus(I2cBusSpeed.Standard),
        address: (byte)I2cCharacterDisplay.Addresses.Grove,
        rows: 2, columns: 16,
        isGroveDisplay: true
    );
}

void TestCharacterDisplay() 
{
    Console.WriteLine("TestCharacterDisplay...");

    display.WriteLine("Hello", 0);

    display.WriteLine("Display", 1);

    Thread.Sleep(1000);
    display.WriteLine("Will delete in", 0);

    int count = 5;
    while(count > 0)
    {
        display.WriteLine($"{count--}", 1);
        Thread.Sleep(500);
    }

    display.ClearLines();
    Thread.Sleep(2000);

    display.WriteLine("Cursor test", 0);

    for (int i = 0; i < display.DisplayConfig.Width; i++)
    {
        display.SetCursorPosition((byte)i, 1);
        display.Write("*");
        Thread.Sleep(100);
        display.SetCursorPosition((byte)i, 1);
        display.Write(" ");
    }

    display.ClearLines();
    display.WriteLine("Complete!", 0);
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/main/Source/Meadow.Foundation.Peripherals/Displays.Lcd.CharacterDisplay/Samples/CharacterDisplay_Sample)

### Wiring Example

<img src="../../API_Assets/Meadow.Foundation.Displays.Lcd.CharacterDisplay/CharacterDisplay_Fritzing.svg" 
    style="width: 60%; display: block; margin-left: auto; margin-right: auto;" />




