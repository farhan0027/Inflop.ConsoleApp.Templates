# Inflop.ConsoleApp.Templates

![Inflop Console App Templates Icon](https://raw.githubusercontent.com/inflop/Inflop.ConsoleApp.Templates/master/src/icon.png)

A comprehensive collection of .NET console application templates with built-in support for Dependency Injection, Configuration (appsettings.json), and Logging (ILogger<T>).

## Requirements

- **.NET SDK 8.0 or later** (LTS recommended)
- Supports .NET 8.0 (LTS), .NET 9.0 (STS), and .NET 10.0 (LTS, latest)

## Features

- ✅ **4 Template Options** - From minimal to enterprise
- ✅ **.NET 8.0+ Support** - Modern .NET with Long-Term Support (net8.0, net9.0, net10.0)
- ✅ **Built-in DI** - Microsoft.Extensions.DependencyInjection
- ✅ **Configuration** - appsettings.json with environment-specific overrides
- ✅ **Logging** - ILogger<T> without third-party dependencies (Serilog optional)
- ✅ **Database Support** - EF Core or Dapper (SQLite, SQL Server, PostgreSQL)
- ✅ **Message Queues** - RabbitMQ, Azure Service Bus, Apache Kafka
- ✅ **HTTP Client** - Typed client with optional Polly resilience (retry, circuit breaker)
- ✅ **Health Checks** - Monitoring for all dependencies (programmatic or ASP.NET endpoint)
- ✅ **CLI Parsing** - 3 library options (System.CommandLine, Spectre.Console, CommandLineParser)
- ✅ **Docker Ready** - Dockerfile + docker-compose with conditional service containers
- ✅ **Async/Await Support** - Choose between async and sync execution patterns via parameter
- ✅ **Selective Generation** - Only includes files for features you enable

---

## Installation

### Install from NuGet Package

```bash
dotnet new install Inflop.ConsoleApp.Templates
```

### Install from Local .nupkg File

```bash
dotnet new install Inflop.ConsoleApp.Templates.1.0.0.nupkg
```

### Verify Installation

```bash
dotnet new list
```

You should see 4 new templates:

- `inflop-simple` - Console App (Simple)
- `inflop-standard` - Console App (Standard)
- `inflop-advanced` - Console App (Advanced)
- `inflop-enterprise` - Console App (Enterprise)

---

## Async/Await Support

All templates support both **asynchronous** and **synchronous** execution patterns via the `--use-async` parameter.

### Template Defaults

- **Template 1 (Simple)**: Synchronous by default (easier for beginners)
- **Templates 2, 3, 4 (Standard, Advanced, Enterprise)**: Asynchronous by default

### Usage Examples

#### Simple Template (sync by default)

```bash
# Create synchronous version (default)
dotnet new inflop-simple -n MyApp

# Create asynchronous version
dotnet new inflop-simple -n MyApp --use-async true
```

#### Standard, Advanced, Enterprise Templates (async by default)

```bash
# Create asynchronous version (default - recommended)
dotnet new inflop-standard -n MyApp
dotnet new inflop-advanced -n MyApp
dotnet new inflop-enterprise -n MyApp

# Create synchronous version
dotnet new inflop-standard -n MyApp --use-async false
dotnet new inflop-advanced -n MyApp --use-async false
dotnet new inflop-enterprise -n MyApp --use-async false
```

### When to Use Async vs Sync?

**Use Async (Recommended for most cases):**
- Database operations (EF Core, Dapper)
- HTTP requests (HttpClient)
- Messaging (RabbitMQ, Azure Service Bus, Kafka)
- File I/O operations
- Any I/O-bound operations

**Use Sync (Only for specific scenarios):**
- CPU-bound operations only
- Simple scripts without I/O
- Learning DI basics without async complexity
- Legacy code integration

**Note:** The synchronous version uses `.GetAwaiter().GetResult()` for I/O operations with warning comments, as true synchronous HTTP/database APIs are not available in modern .NET.

---

## Template Overview

### 1. Console App (Simple) - `inflop-simple`

**Best for:** Simple console applications and CLI tools

**Features:**

- Manual DI container setup (no Generic Host)
- Inline service registration in Program.cs (no extension methods)
- ILogger<T> with console provider
- appsettings.json support
- **3 NuGet packages** (absolute minimum for base template)
- Direct service registration for maximum clarity
- More control over lifecycle

**Usage:**

```bash
dotnet new inflop-simple -n MyTool
dotnet new inflop-simple -n MyTool -F net8.0
```

**Project Structure:**

```
MyTool/
├── Program.cs (all DI setup + configuration)
├── appsettings.json
├── Services/
│   └── MyService.cs
└── MyTool.csproj
```

**Note:** Additional folders (Data/, Infrastructure/, Messaging/, Health/, Extensions/) are generated only when corresponding features are enabled via parameters.

---

### 2. Console App (Standard) - `inflop-standard`

**Best for:** Standard console applications with medium complexity

**Features:**

- Generic Host pattern (like ASP.NET Core)
- Built-in ILogger<T>
- appsettings.json with Development override
- BackgroundService pattern
- **2 NuGet packages** (minimal dependencies)

**Usage:**

```bash
dotnet new inflop-standard -n MyApp
dotnet new inflop-standard -n MyApp -F net8.0
```

**Project Structure:**
```
MyApp/
├── Program.cs
├── appsettings.json
├── appsettings.Development.json
├── Services/
│   ├── AppService.cs
│   └── AppBackgroundService.cs
├── Extensions/ (conditional - only with enabled features)
│   ├── DatabaseExtensions.cs
│   ├── HttpClientExtensions.cs
│   ├── HealthChecksExtensions.cs
│   ├── MessagingExtensions.cs
│   └── CommandLineExtensions.cs
└── MyApp.csproj
```

---

### 3. Console App (Advanced) - `inflop-advanced`

**Best for:** Modern .NET applications with minimal boilerplate

**Features:**

- Top-level statements (C# 10+)
- Generic Host pattern
- Extension methods for clean DI registration
- Modern C# features (primary constructors)
- **2 NuGet packages**

**Usage:**

```bash
dotnet new inflop-advanced -n ModernApp
dotnet new inflop-advanced -n ModernApp -F net8.0
```

**Project Structure:**

```
ModernApp/
├── Program.cs (top-level statements)
├── appsettings.json
├── appsettings.Development.json
├── Configuration/ (conditional - with enabled features)
│   ├── DatabaseOptions.cs
│   ├── HttpClientOptions.cs
│   └── MessagingOptions.cs
├── Extensions/
│   ├── ServiceExtensions.cs
│   ├── DatabaseExtensions.cs (conditional)
│   ├── HttpClientExtensions.cs (conditional)
│   ├── HealthChecksExtensions.cs (conditional)
│   ├── MessagingExtensions.cs (conditional)
│   └── CommandLineExtensions.cs (conditional)
├── Services/
│   ├── AppService.cs
│   └── AppWorker.cs
└── ModernApp.csproj
```

---

### 4. Console App (Enterprise) - `inflop-enterprise`

**Best for:** Enterprise applications with complex requirements

**Features:**

- Strongly-typed configuration (Options pattern)
- Multiple background workers
- Environment-specific configuration (Development, Production)
- Structured logging with configurable levels
- Retry logic and error handling patterns
- **5 NuGet packages**

**Usage:**

```bash
dotnet new inflop-enterprise -n EnterpriseApp
dotnet new inflop-enterprise -n EnterpriseApp -F net8.0
```

**Project Structure:**

```
EnterpriseApp/
├── Program.cs
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Production.json
├── Configuration/
│   ├── AppSettings.cs
│   ├── WorkerSettings.cs
│   └── ServiceConfiguration.cs
├── Extensions/ (conditional - only with enabled features)
│   ├── DatabaseExtensions.cs
│   ├── HttpClientExtensions.cs
│   ├── HealthChecksExtensions.cs
│   ├── MessagingExtensions.cs
│   └── CommandLineExtensions.cs
├── Services/
│   ├── IAppService.cs
│   ├── AppService.cs
│   ├── IDataProcessor.cs
│   ├── DataProcessor.cs
│   ├── PrimaryWorker.cs
│   └── SecondaryWorker.cs
└── EnterpriseApp.csproj
```

---

## Advanced Template Parameters

All templates support optional parameters for adding enterprise features:

### Database Access (`--add-database`)

Add database support with Entity Framework Core or Dapper:

```bash
# EF Core with SQLite (default)
dotnet new inflop-advanced -n MyApp --add-database efcore

# Dapper with PostgreSQL
dotnet new inflop-advanced -n MyApp --add-database dapper --database-type postgres

# EF Core with SQL Server
dotnet new inflop-enterprise -n MyApp --add-database efcore --database-type sqlserver
```

**Options:**
- `none` - No database (default)
- `dapper` - Lightweight ORM (Factory Pattern)
- `efcore` - Entity Framework Core

**Database Types (`--database-type`):**
- `sqlite` - File-based, no external dependencies (default)
- `sqlserver` - SQL Server
- `postgres` - PostgreSQL

**What's Included:**
- ✅ Repository pattern (`IExampleRepository`)
- ✅ Example entity (`ExampleEntity`)
- ✅ Connection factories (Dapper) or DbContext (EF Core)
- ✅ Configured connection strings in `appsettings.json`

---

### HTTP Client (`--add-httpclient`)

Add typed HttpClient with optional Polly resilience:

```bash
# Basic HttpClient
dotnet new inflop-advanced -n MyApp --add-httpclient basic

# With Polly (retry + circuit breaker)
dotnet new inflop-enterprise -n MyApp --add-httpclient with-polly
```

**Options:**
- `none` - No HTTP client (default)
- `basic` - Basic typed HttpClient
- `with-polly` - Resilient client with retry, circuit breaker, and timeout

**What's Included:**
- ✅ `IApiClient` interface and implementation
- ✅ Configured base URL and timeout
- ✅ Polly policies: exponential retry (3x), circuit breaker, timeout

---

### Health Checks (`--add-healthchecks`)

Add health monitoring for your application and dependencies:

```bash
# Programmatic health checks
dotnet new inflop-advanced -n MyApp --add-healthchecks basic

# With ASP.NET endpoint at /health
dotnet new inflop-enterprise -n MyApp --add-healthchecks aspnet
```

**Options:**
- `none` - No health checks (default)
- `basic` - Programmatic health checks
- `aspnet` - Health checks with HTTP endpoint

**What's Included:**
- ✅ Custom health check example
- ✅ Automatic checks for database, messaging, and HTTP dependencies
- ✅ ASP.NET endpoint at `/health` (aspnet mode)

---

### Message Queue (`--add-messaging`)

Add message consumer/publisher for distributed systems:

```bash
# RabbitMQ
dotnet new inflop-advanced -n Worker --add-messaging rabbitmq

# Azure Service Bus
dotnet new inflop-enterprise -n Worker --add-messaging azureservicebus

# Apache Kafka
dotnet new inflop-advanced -n Worker --add-messaging kafka
```

**Options:**
- `none` - No messaging (default)
- `rabbitmq` - RabbitMQ
- `azureservicebus` - Azure Service Bus
- `kafka` - Apache Kafka

**What's Included:**
- ✅ `IMessageConsumer` and `IMessagePublisher` interfaces
- ✅ Provider-specific implementations
- ✅ Continuous consumer pattern
- ✅ ACK/NACK handling and retry logic
- ✅ Configuration in `appsettings.json`

---

### Command-Line Parsing (`--add-commandline`)

Add argument parsing with your choice of library:

```bash
# Microsoft official (System.CommandLine)
dotnet new inflop-simple -n MyTool --add-commandline system-commandline

# Rich console UI (Spectre.Console)
dotnet new inflop-advanced -n MyTool --add-commandline spectre-console

# Attribute-based (CommandLineParser)
dotnet new inflop-simple -n MyTool --add-commandline command-line-parser
```

**Options:**
- `none` - No CLI parsing (default)
- `system-commandline` - System.CommandLine (Microsoft)
- `spectre-console` - Spectre.Console (rich UI)
- `command-line-parser` - CommandLineParser (attributes)

**What's Included:**
- ✅ `CommandLineOptions` class
- ✅ Parsing extensions for each library
- ✅ Example options: `--name`, `--verbose`
- ✅ Integration in `Program.cs`

---

### Docker Support (`--add-docker`)

Add containerization with Docker:

```bash
dotnet new inflop-advanced -n MyApp --add-docker
```

**What's Included:**
- ✅ `Dockerfile` with multi-stage build
- ✅ `.dockerignore` file
- ✅ `docker-compose.yml` with conditional services:
  - Database containers (SQL Server, PostgreSQL)
  - Message queue containers (RabbitMQ, Kafka + Zookeeper)

---

### Structured Logging (`--add-serilog`)

Add Serilog for structured logging:

```bash
dotnet new inflop-advanced -n MyApp --add-serilog
```

**What's Included:**
- ✅ Serilog with Console and File sinks
- ✅ Configured via `appsettings.json`
- ✅ Structured logging with context

---

## Real-World Examples

### Microservice Worker with Database and Messaging

```bash
dotnet new inflop-advanced -n OrderProcessor \
  --add-database efcore -D postgres \
  --add-messaging rabbitmq \
  --add-healthchecks aspnet \
  --add-docker \
  --add-serilog
```

**Generates:**
- PostgreSQL with EF Core repository
- RabbitMQ consumer/publisher
- Health checks with `/health` endpoint
- Docker Compose with Postgres + RabbitMQ
- Structured logging (Serilog)

---

### API Client with Resilience

```bash
dotnet new inflop-enterprise -n ApiClient \
  --add-httpclient with-polly \
  --add-commandline system-commandline \
  --add-healthchecks basic \
  --add-serilog
```

**Generates:**
- Resilient HTTP client (retry + circuit breaker)
- Command-line argument parsing
- Health checks for API endpoint
- Serilog logging

---

### Data Pipeline

```bash
dotnet new inflop-advanced -n DataPipeline \
  --add-database dapper -D sqlserver \
  --add-httpclient with-polly \
  --add-messaging kafka \
  --add-docker
```

**Generates:**
- SQL Server with Dapper
- Resilient HTTP client
- Kafka consumer/publisher
- Docker Compose

---

## Comparison Matrix

| Feature | Simple | Standard | Advanced | Enterprise |
|---------|------------|-------------|---------|------------|
| **Template ID** | `inflop-simple` | `inflop-standard` | `inflop-advanced` | `inflop-enterprise` |
| **Base NuGet Packages** | 3 | 2 | 2 | 5 |
| **DI Container** | Manual ServiceCollection | Generic Host | Generic Host | Generic Host |
| **Code Style** | Traditional | Traditional | Top-Level Statements | Traditional |
| **Service Registration** | Inline in Program.cs | Extension methods | Extension methods | Centralized config class |
| **ILogger<T>** | ✅ | ✅ | ✅ | ✅ |
| **appsettings.json** | ✅ | ✅ | ✅ | ✅ |
| **Environment Config** | ❌ | Dev | Dev | Dev + Prod |
| **Strongly-Typed Config** | ❌ | ❌ | Partial (Options) | ✅ (Options pattern) |
| **Background Workers** | ❌ | 1 (BackgroundService) | 1 (Worker) | 2 (Primary + Secondary) |
| **Retry Logic** | ❌ | ❌ | ❌ | ✅ |
| **Default Async** | ❌ (sync) | ✅ (async) | ✅ (async) | ✅ (async) |
| **Complexity** | Very Low | Low | Low-Medium | Medium-High |
| **Best For** | CLI tools, simple scripts | Standard apps, services | Modern .NET apps | Enterprise solutions |

### Advanced Parameters (All Templates)

| Parameter | Options | Description |
|-----------|---------|-------------|
| `--add-database` | none, dapper, efcore | Database access layer |
| `--database-type` | sqlite, sqlserver, postgres | Database engine |
| `--add-httpclient` | none, basic, with-polly | Typed HTTP client |
| `--add-healthchecks` | none, basic, aspnet | Health monitoring |
| `--add-messaging` | none, rabbitmq, azureservicebus, kafka | Message queue |
| `--add-commandline` | none, system-commandline, spectre-console, command-line-parser | CLI parsing |
| `--add-docker` | true, false | Docker support |
| `--add-serilog` | true, false | Structured logging |
| `--use-async` | true, false | Use async/await pattern |

---

## Multi-Version Support

All templates support .NET 8.0 (LTS), 9.0 (STS), and 10.0 (LTS, latest). Specify the target framework during creation:

```bash
# Default (net8.0 LTS - Recommended)
dotnet new inflop-simple -n MyApp

# .NET 8.0 (LTS) - Explicitly specified
dotnet new inflop-simple -n MyApp -F net8.0

# .NET 9.0 (STS)
dotnet new inflop-simple -n MyApp -F net9.0

# .NET 10.0 (LTS, Latest)
dotnet new inflop-simple -n MyApp -F net10.0
```

**Note:** Templates require .NET 8.0 or later. .NET 6.0 and 7.0 are no longer supported as they have reached end-of-life.

---

## Quick Start Examples

### Example 1: Create and Run Basic Console App

```bash
# Create project
dotnet new inflop-simple -n MyTool

# Navigate to project
cd MyTool

# Run application
dotnet run
```

### Example 2: Create Hosted DI Console App

```bash
# Create project with .NET 8.0
dotnet new inflop-standard -n MyApp -F net8.0

# Navigate and run
cd MyApp
dotnet run
```

### Example 3: Create Modern Console App

```bash
# Create project
dotnet new inflop-advanced -n ModernApp

# Navigate and run in Development
cd ModernApp
dotnet run --environment Development
```

### Example 4: Create Enterprise Console App

```bash
# Create project with .NET 8.0
dotnet new inflop-enterprise -n MyEnterpriseApp -F net8.0

# Navigate and run
cd MyEnterpriseApp

# Run in Development
dotnet run --environment Development

# Run in Production
dotnet run --environment Production
```

---

## Building the Package

If you want to build the package yourself:

```bash
# Navigate to template root
cd Inflop.ConsoleApp.Templates

# Pack the template
dotnet pack -c Release

# Install locally
dotnet new install bin/Release/Inflop.ConsoleApp.Templates.1.0.0.nupkg
```

---

## Uninstalling Templates

```bash
# Uninstall by package name
dotnet new uninstall Inflop.ConsoleApp.Templates

# Uninstall by path (if installed from .nupkg)
dotnet new uninstall /path/to/Inflop.ConsoleApp.Templates.1.0.0.nupkg
```

---

## Common Configuration Patterns

### Accessing Configuration in Services

```csharp
// Inject IConfiguration
public class MyService
{
    private readonly IConfiguration _configuration;

    public MyService(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public void DoWork()
    {
        var setting = _configuration["MySetting"];
        var nestedSetting = _configuration["Section:NestedSetting"];
    }
}
```

### Using Strongly-Typed Configuration (Enterprise Template)

```csharp
// Configuration class
public class MySettings
{
    public string ApiUrl { get; set; }
    public int Timeout { get; set; }
}

// Register in Program.cs
builder.Services.Configure<MySettings>(
    builder.Configuration.GetSection("MySettings"));

// Inject in service
public class MyService
{
    private readonly MySettings _settings;

    public MyService(IOptions<MySettings> settings)
    {
        _settings = settings.Value;
    }
}
```

---

## Environment-Specific Configuration

### Setting the Environment

```bash
# Via environment variable
export DOTNET_ENVIRONMENT=Development
dotnet run

# Via command-line argument
dotnet run --environment Production

# Via launchSettings.json (Development only)
# Edit Properties/launchSettings.json
```

### Configuration File Priority

1. `appsettings.json` - Base configuration
2. `appsettings.{Environment}.json` - Environment-specific overrides
3. Environment variables
4. Command-line arguments

---

## Adding Serilog (Optional)

If you want to use Serilog for advanced logging:

```bash
# Add Serilog packages
dotnet add package Serilog
dotnet add package Serilog.Extensions.Hosting
dotnet add package Serilog.Sinks.Console
```

```csharp
// In Program.cs
using Serilog;

Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Information()
    .WriteTo.Console()
    .CreateLogger();

builder.Logging.ClearProviders();
builder.Logging.AddSerilog(Log.Logger, dispose: true);
```

---

## Design Principles

All templates follow modern software development practices:

- **Dependency Injection** - Loose coupling and testability
- **SOLID Principles** - Single Responsibility, Open/Closed, Interface Segregation, Dependency Inversion
- **Configuration over Code** - Settings in appsettings.json
- **Clean Code** - Readable and maintainable

---

## Support & Feedback

- **GitHub Issues:** [https://github.com/inflop/Inflop.ConsoleApp.Templates/issues](https://github.com/inflop/Inflop.ConsoleApp.Templates/issues)
- **License:** MIT

---

## Version History

### 1.0.0 (Current Release)

- ✅ 4 console application templates (Simple, Standard, Advanced, Enterprise)
- ✅ Multi-version support (.NET 8.0, 9.0, 10.0)
- ✅ 8 advanced parameters for enterprise features:
  - Database access (EF Core, Dapper) with 3 DB engines
  - Message queues (RabbitMQ, Azure Service Bus, Kafka)
  - HTTP client with Polly resilience
  - Health checks with automatic dependency monitoring
  - CLI parsing (3 library options)
  - Docker + docker-compose
  - Serilog structured logging
- ✅ Selective file generation (only includes what you need)

---

## License

MIT License - See LICENSE file for details

---

## Acknowledgments

This project was created with the support of [Claude Code](https://claude.ai/code) - Anthropic's official CLI for Claude.
