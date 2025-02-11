---
uid: Meadow.Foundation.FeatherWings.CharlieWing
remarks: *content
---

| CharlieWing | |
|--------|--------|
| Status | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" alt="Status badge: working" /> |
| Source code | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation.FeatherWings/tree/main/Source/CharlieWing) |
| Datasheet(s) | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation.FeatherWings/tree/main/Source/CharlieWing/Datasheet) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.FeatherWings.CharlieWing/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.FeatherWings.CharlieWing.svg?label=Meadow.Foundation.FeatherWings.CharlieWing" alt="NuGet Gallery for CharlieWing" /></a> |

### Code Example

```csharp
CharlieWing charlieWing;
MicroGraphics graphics;

public MeadowApp()
{
    Console.WriteLine("Initializing ...");

    charlieWing = new CharlieWing(Device.CreateI2cBus());
    charlieWing.Clear();

    graphics = new MicroGraphics(charlieWing);
    graphics.CurrentFont = new Font4x8();

    graphics.DrawText(0, 0, "F7");
    graphics.Show();
}

```

[Sample project(s) available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation.FeatherWings/tree/main/Source/CharlieWing/Samples/CharlieWing_Sample)

