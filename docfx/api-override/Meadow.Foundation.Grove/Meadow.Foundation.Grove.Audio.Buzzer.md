---
uid: Meadow.Foundation.Grove.Audio.Buzzer
remarks: *content
---

| Buzzer | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation.Grove/tree/main/Source/Buzzer) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.Grove.Audio.Buzzer/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.Grove.Audio.Buzzer.svg?label=Meadow.Foundation.Grove.Audio.Buzzer" alt="NuGet Gallery for Buzzer" /></a> |

### Code Example

```csharp
readonly Buzzer buzzer;

public MeadowApp()
{
    Console.WriteLine("Initialize hardware...");

    buzzer = new Buzzer(Device.CreatePwmPort(Device.Pins.D13));

    _ = PlayTriad();
}

async Task PlayTriad()
{
    for (int i = 0; i < 5; i++)
    {
        Console.WriteLine("Playing A major triad starting at A4");
        await buzzer.PlayTone(440, 500); //A
        await buzzer.PlayTone(554.37f, 500); //C#
        await buzzer.PlayTone(659.25f, 500); //E
        
        await Task.Delay(2500);
    }
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation.Grove/tree/main/Source/Buzzer/Sample/Buzzer_Sample)

### Wiring Example

| Buzzer | Meadow Pin |
|--------|------------|
| GND    | GND        |
| VCC    | 3.3V       |
| RX     | D01        |
| TX     | D00        |