# Setup Logging

- [Setup Logging](#setup-logging)
  - [Default Logging Setup](#default-logging-setup)
  - [Serilog Setup (Logging Provider)](#serilog-setup-logging-provider)

## Default Logging Setup

<details>

<summary>JSON</summary>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "Inventory": "Critical"
    },
    "Console": {
      "FormatterName": "simple", // or, "json"
      "FormatterOptions": {
        "SingleLine": true,
        "IncludeScopes": true,
        "TimestampFormat": "HH:mm:ss ",
        "UseUtcTimestamp": true,
        "JsonWriterOptions": {
          "Indented": true
        }
      }
    },
    "Debug": {
      "LogLevel": {
        "Default": "Critical"
      }
    }
  }
}
```

</details>

## Serilog Setup (Logging Provider)

[Serilog](https://serilog.net) is a structured data logging framework.

<details><summary>C# Code</summary>

```cs
builder.Logging.ClearProviders();
builder.Host.UseSerilog((ctx, config) =>
{
  config
    .MinimumLevel.Debug()
    .Enrich.FromLogContext()
    .Enrich.WithExceptionDetails() // package: Serilog.Exceptions
    .Enrich.With<ActivityEnricher>() // package: Serilog.Enrichers.Span
    .WriteTo.Console()
    .WriteTo.Seq(serverUrl: "http://localhost:5341")
    .ReadFrom.Configuration(ctx.Configuration);
});
```

</details>
