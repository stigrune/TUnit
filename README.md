![](assets/banner.png)

# 🚀 The Modern Testing Framework for .NET

**TUnit** is a next-generation testing framework for C# that outpaces traditional frameworks with **source-generated tests**, **parallel execution by default**, and **Native AOT support**. Built on the modern Microsoft.Testing.Platform, TUnit delivers faster test runs, better developer experience, and unmatched flexibility.

<div align="center">

[![thomhurst%2FTUnit | Trendshift](https://trendshift.io/api/badge/repositories/11781)](https://trendshift.io/repositories/11781)


[![Codacy Badge](https://api.codacy.com/project/badge/Grade/a8231644d844435eb9fd15110ea771d8)](https://app.codacy.com/gh/thomhurst/TUnit?utm_source=github.com&utm_medium=referral&utm_content=thomhurst/TUnit&utm_campaign=Badge_Grade)![GitHub Repo stars](https://img.shields.io/github/stars/thomhurst/TUnit) ![GitHub Issues or Pull Requests](https://img.shields.io/github/issues-closed-raw/thomhurst/TUnit)
 [![GitHub Sponsors](https://img.shields.io/github/sponsors/thomhurst)](https://github.com/sponsors/thomhurst) [![nuget](https://img.shields.io/nuget/v/TUnit.svg)](https://www.nuget.org/packages/TUnit/) [![NuGet Downloads](https://img.shields.io/nuget/dt/TUnit)](https://www.nuget.org/packages/TUnit/) ![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/thomhurst/TUnit/dotnet.yml) ![GitHub last commit (branch)](https://img.shields.io/github/last-commit/thomhurst/TUnit/main) ![License](https://img.shields.io/github/license/thomhurst/TUnit)

</div>

## ⚡ Why Choose TUnit?

| Feature | Traditional Frameworks | **TUnit** |
|---------|----------------------|-----------|
| Test Discovery | ❌ Runtime reflection | ✅ **Compile-time generation** |
| Execution Speed | ❌ Sequential by default | ✅ **Parallel by default** |
| Modern .NET | ⚠️ Limited AOT support | ✅ **Full Native AOT & trimming** |
| Test Dependencies | ❌ Not supported | ✅ **`[DependsOn]` chains** |
| Resource Management | ❌ Manual lifecycle | ✅ **Intelligent cleanup** |

⚡ **Parallel by Default** - Tests run concurrently with intelligent dependency management

🎯 **Compile-Time Discovery** - Know your test structure before runtime

🔧 **Modern .NET Ready** - Native AOT, trimming, and latest .NET features

🎭 **Extensible** - Customize data sources, attributes, and test behavior

---

<div align="center">

## 📚 **[Complete Documentation & Learning Center](https://tunit.dev)**

**🚀 New to TUnit?** Start with our **[Getting Started Guide](https://tunit.dev/docs/getting-started/installation)**

**🔄 Migrating?** See our **[Migration Guides](https://tunit.dev/docs/migration/xunit)**

**🎯 Advanced Features?** Explore **[Data-Driven Testing](https://tunit.dev/docs/test-authoring/arguments)**, **[Test Dependencies](https://tunit.dev/docs/test-authoring/depends-on)**, and **[Parallelism Control](https://tunit.dev/docs/parallelism/not-in-parallel)**

</div>

---

## 🏁 Quick Start

### Using the Project Template (Recommended)
```bash
dotnet new install TUnit.Templates
dotnet new TUnit -n "MyTestProject"
```

### Manual Installation
```bash
dotnet add package TUnit --prerelease
```

📖 **[📚 Complete Documentation & Guides](https://tunit.dev)** - Everything you need to master TUnit

## ✨ Key Features

<table>
<tr>
<td width="50%">

**🚀 Performance & Modern Platform**
- 🔥 Source-generated tests (no reflection)
- ⚡ Parallel execution by default
- 🚀 Native AOT & trimming support
- 📈 Optimized for performance

</td>
<td width="50%">

**🎯 Advanced Test Control**
- 🔗 Test dependencies with `[DependsOn]`
- 🎛️ Parallel limits & custom scheduling
- 🛡️ Built-in analyzers & compile-time checks
- 🎭 Custom attributes & extensible conditions

</td>
</tr>
<tr>
<td>

**📊 Rich Data & Assertions**
- 📋 Multiple data sources (`[Arguments]`, `[Matrix]`, `[ClassData]`)
- ✅ Fluent async assertions
- 🔄 Smart retry logic & conditional execution
- 📝 Rich test metadata & context

</td>
<td>

**🔧 Developer Experience**
- 💉 Full dependency injection support
- 🪝 Comprehensive lifecycle hooks
- 🎯 IDE integration (VS, Rider, VS Code)
- 📚 Extensive documentation & examples

</td>
</tr>
</table>

## 📝 Simple Test Example

```csharp
[Test]
public async Task User_Creation_Should_Set_Timestamp()
{
    // Arrange
    var userService = new UserService();

    // Act
    var user = await userService.CreateUserAsync("john.doe@example.com");

    // Assert - TUnit's fluent assertions
    await Assert.That(user.CreatedAt)
        .IsEqualTo(DateTime.Now)
        .Within(TimeSpan.FromMinutes(1));

    await Assert.That(user.Email)
        .IsEqualTo("john.doe@example.com");
}
```

## 🎯 Data-Driven Testing

```csharp
[Test]
[Arguments("user1@test.com", "ValidPassword123")]
[Arguments("user2@test.com", "AnotherPassword456")]
[Arguments("admin@test.com", "AdminPass789")]
public async Task User_Login_Should_Succeed(string email, string password)
{
    var result = await authService.LoginAsync(email, password);
    await Assert.That(result.IsSuccess).IsTrue();
}

// Matrix testing - tests all combinations
[Test]
[MatrixDataSource]
public async Task Database_Operations_Work(
    [Matrix("Create", "Update", "Delete")] string operation,
    [Matrix("User", "Product", "Order")] string entity)
{
    await Assert.That(await ExecuteOperation(operation, entity))
        .IsTrue();
}
```

## 🔗 Advanced Test Orchestration

```csharp
[Before(Class)]
public static async Task SetupDatabase(ClassHookContext context)
{
    await DatabaseHelper.InitializeAsync();
}

[Test, DisplayName("Register a new account")]
[MethodDataSource(nameof(GetTestUsers))]
public async Task Register_User(string username, string password)
{
    // Test implementation
}

[Test, DependsOn(nameof(Register_User))]
[Retry(3)] // Retry on failure
public async Task Login_With_Registered_User(string username, string password)
{
    // This test runs after Register_User completes
}

[Test]
[ParallelLimit<LoadTestParallelLimit>] // Custom parallel control
[Repeat(100)] // Run 100 times
public async Task Load_Test_Homepage()
{
    // Performance testing
}

// Custom attributes
[Test, WindowsOnly, RetryOnHttpError(5)]
public async Task Windows_Specific_Feature()
{
    // Platform-specific test with custom retry logic
}

public class LoadTestParallelLimit : IParallelLimit
{
    public int Limit => 10; // Limit to 10 concurrent executions
}
```

## 🔧 Smart Test Control

```csharp
// Custom conditional execution
public class WindowsOnlyAttribute : SkipAttribute
{
    public WindowsOnlyAttribute() : base("Windows only test") { }

    public override Task<bool> ShouldSkip(TestContext testContext)
        => Task.FromResult(!OperatingSystem.IsWindows());
}

// Custom retry logic
public class RetryOnHttpErrorAttribute : RetryAttribute
{
    public RetryOnHttpErrorAttribute(int times) : base(times) { }

    public override Task<bool> ShouldRetry(TestInformation testInformation,
        Exception exception, int currentRetryCount)
        => Task.FromResult(exception is HttpRequestException { StatusCode: HttpStatusCode.ServiceUnavailable });
}
```

## 🎯 Perfect For Every Testing Scenario

<table>
<tr>
<td width="33%">

### 🧪 **Unit Testing**
```csharp
[Test]
[Arguments(1, 2, 3)]
[Arguments(5, 10, 15)]
public async Task Calculate_Sum(int a, int b, int expected)
{
    await Assert.That(Calculator.Add(a, b))
        .IsEqualTo(expected);
}
```
**Fast, isolated, and reliable**

</td>
<td width="33%">

### 🔗 **Integration Testing**
```csharp
[Test, DependsOn(nameof(CreateUser))]
public async Task Login_After_Registration()
{
    // Runs after CreateUser completes
    var result = await authService.Login(user);
    await Assert.That(result.IsSuccess).IsTrue();
}
```
**Stateful workflows made simple**

</td>
<td width="33%">

### ⚡ **Load Testing**
```csharp
[Test]
[ParallelLimit<LoadTestLimit>]
[Repeat(1000)]
public async Task API_Handles_Concurrent_Requests()
{
    await Assert.That(await httpClient.GetAsync("/api/health"))
        .HasStatusCode(HttpStatusCode.OK);
}
```
**Built-in performance testing**

</td>
</tr>
</table>

## 🚀 What Makes TUnit Different?

### **Compile-Time Intelligence**
Tests are discovered at build time, not runtime - enabling faster discovery, better IDE integration, and precise resource lifecycle management.

### **Parallel-First Architecture**
Built for concurrency from day one with `[DependsOn]` for test chains, `[ParallelLimit]` for resource control, and intelligent scheduling.

### **Extensible by Design**
The `DataSourceGenerator<T>` pattern and custom attribute system let you extend TUnit's capabilities without modifying core framework code.

## 🏆 Community & Ecosystem

<div align="center">

**🌟 Join thousands of developers modernizing their testing**

[![Downloads](https://img.shields.io/nuget/dt/TUnit?label=Downloads&color=blue)](https://www.nuget.org/packages/TUnit/)
[![Contributors](https://img.shields.io/github/contributors/thomhurst/TUnit?label=Contributors)](https://github.com/thomhurst/TUnit/graphs/contributors)
[![Discussions](https://img.shields.io/github/discussions/thomhurst/TUnit?label=Discussions)](https://github.com/thomhurst/TUnit/discussions)

</div>

### 🤝 **Active Community**
- 📚 **[Official Documentation](https://tunit.dev)** - Comprehensive guides, tutorials, and API reference
- 💬 **[GitHub Discussions](https://github.com/thomhurst/TUnit/discussions)** - Get help and share ideas
- 🐛 **[Issue Tracking](https://github.com/thomhurst/TUnit/issues)** - Report bugs and request features
- 📢 **[Release Notes](https://github.com/thomhurst/TUnit/releases)** - Stay updated with latest improvements

## 🛠️ IDE Support

TUnit works seamlessly across all major .NET development environments:

### Visual Studio (2022 17.13+)
✅ **Fully supported** - No additional configuration needed for latest versions

⚙️ **Earlier versions**: Enable "Use testing platform server mode" in Tools > Manage Preview Features

### JetBrains Rider
✅ **Fully supported**

⚙️ **Setup**: Enable "Testing Platform support" in Settings > Build, Execution, Deployment > Unit Testing > VSTest

### Visual Studio Code
✅ **Fully supported**

⚙️ **Setup**: Install [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) and enable "Use Testing Platform Protocol"

### Command Line
✅ **Full CLI support** - Works with `dotnet test`, `dotnet run`, and direct executable execution

## 📦 Package Options

| Package | Use Case |
|---------|----------|
| **`TUnit`** | ⭐ **Start here** - Complete testing framework (includes Core + Engine + Assertions) |
| **`TUnit.Core`** | 📚 Test libraries and shared components (no execution engine) |
| **`TUnit.Engine`** | 🚀 Test execution engine and adapter (for test projects) |
| **`TUnit.Assertions`** | ✅ Standalone assertions (works with any test framework) |
| **`TUnit.Playwright`** | 🎭 Playwright integration with automatic lifecycle management |

## 🎯 Migration from Other Frameworks

**Coming from NUnit or xUnit?** TUnit maintains familiar syntax while adding modern capabilities:

```csharp
// Enhanced with TUnit's advanced features
[Test]
[Arguments("value1")]
[Arguments("value2")]
[Retry(3)]
[ParallelLimit<CustomLimit>]
public async Task Modern_TUnit_Test(string value) { }
```

📖 **Need help migrating?** Check our detailed **[Migration Guides](https://tunit.dev/docs/migration/xunit)** with step-by-step instructions for xUnit, NUnit, and MSTest.


## 💡 Current Status

The API is mostly stable, but may have some changes based on feedback or issues before v1.0 release.

---

<div align="center">

## 🚀 Ready to Experience the Future of .NET Testing?

### ⚡ **Start in 30 Seconds**

```bash
# Create a new test project with examples
dotnet new install TUnit.Templates && dotnet new TUnit -n "MyAwesomeTests"

# Or add to existing project
dotnet add package TUnit --prerelease
```

### 🎯 **Why Wait? Join the Movement**

<table>
<tr>
<td align="center" width="25%">

### 📈 **Performance**
**Optimized execution**
**Parallel by default**
**Zero reflection overhead**

</td>
<td align="center" width="25%">

### 🔮 **Future-Ready**
**Native AOT support**
**Latest .NET features**
**Source generation**

</td>
<td align="center" width="25%">

### 🛠️ **Developer Experience**
**Compile-time checks**
**Rich IDE integration**
**Intelligent debugging**

</td>
<td align="center" width="25%">

### 🎭 **Flexibility**
**Test dependencies**
**Custom attributes**
**Extensible architecture**

</td>
</tr>
</table>

---

**📖 Learn More**: [tunit.dev](https://tunit.dev) | **💬 Get Help**: [GitHub Discussions](https://github.com/thomhurst/TUnit/discussions) | **⭐ Show Support**: [Star on GitHub](https://github.com/thomhurst/TUnit)

*TUnit is actively developed and production-ready. Join our growing community of developers who've made the switch!*

</div>

## Performance Benchmark

### Scenario: Building the test project

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method       | Version | Mean    | Error    | StdDev   | Median  |
|------------- |-------- |--------:|---------:|---------:|--------:|
| Build_TUnit  | 0.57.24 | 1.804 s | 0.1704 s | 0.4945 s | 1.641 s |
| Build_NUnit  | 4.4.0   | 1.803 s | 0.1368 s | 0.3880 s | 1.769 s |
| Build_xUnit  | 2.9.3   | 1.380 s | 0.0751 s | 0.2106 s | 1.332 s |
| Build_MSTest | 3.10.4  | 1.492 s | 0.1028 s | 0.3030 s | 1.462 s |



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method       | Version | Mean    | Error    | StdDev   | Median  |
|------------- |-------- |--------:|---------:|---------:|--------:|
| Build_TUnit  | 0.57.24 | 1.824 s | 0.0364 s | 0.0619 s | 1.811 s |
| Build_NUnit  | 4.4.0   | 1.514 s | 0.0194 s | 0.0172 s | 1.511 s |
| Build_xUnit  | 2.9.3   | 1.534 s | 0.0156 s | 0.0146 s | 1.532 s |
| Build_MSTest | 3.10.4  | 1.531 s | 0.0295 s | 0.0276 s | 1.521 s |



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method       | Version | Mean    | Error    | StdDev   | Median  |
|------------- |-------- |--------:|---------:|---------:|--------:|
| Build_TUnit  | 0.57.24 | 1.889 s | 0.0377 s | 0.0630 s | 1.884 s |
| Build_NUnit  | 4.4.0   | 1.570 s | 0.0160 s | 0.0134 s | 1.574 s |
| Build_xUnit  | 2.9.3   | 1.563 s | 0.0241 s | 0.0225 s | 1.563 s |
| Build_MSTest | 3.10.4  | 1.569 s | 0.0177 s | 0.0157 s | 1.572 s |


### Scenario: Tests focused on assertion performance and validation

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean | Error | StdDev | Median |
|---------- |-------- |-----:|------:|-------:|-------:|
| TUnit_AOT | 0.57.24 |   NA |    NA |     NA |     NA |
| TUnit     | 0.57.24 |   NA |    NA |     NA |     NA |
| NUnit     | 4.4.0   |   NA |    NA |     NA |     NA |
| xUnit     | 2.9.3   |   NA |    NA |     NA |     NA |
| MSTest    | 3.10.4  |   NA |    NA |     NA |     NA |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.NUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.xUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean | Error | StdDev | Median |
|---------- |-------- |-----:|------:|-------:|-------:|
| TUnit_AOT | 0.57.24 |   NA |    NA |     NA |     NA |
| TUnit     | 0.57.24 |   NA |    NA |     NA |     NA |
| NUnit     | 4.4.0   |   NA |    NA |     NA |     NA |
| xUnit     | 2.9.3   |   NA |    NA |     NA |     NA |
| MSTest    | 3.10.4  |   NA |    NA |     NA |     NA |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.NUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.xUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean | Error | StdDev | Median |
|---------- |-------- |-----:|------:|-------:|-------:|
| TUnit_AOT | 0.57.24 |   NA |    NA |     NA |     NA |
| TUnit     | 0.57.24 |   NA |    NA |     NA |     NA |
| NUnit     | 4.4.0   |   NA |    NA |     NA |     NA |
| xUnit     | 2.9.3   |   NA |    NA |     NA |     NA |
| MSTest    | 3.10.4  |   NA |    NA |     NA |     NA |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.NUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.xUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)


### Scenario: Tests running asynchronous operations and async/await patterns

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean       | Error     | StdDev    | Median     |
|---------- |-------- |-----------:|----------:|----------:|-----------:|
| TUnit_AOT | 0.57.24 |   213.6 ms |  17.84 ms |  52.33 ms |   209.8 ms |
| TUnit     | 0.57.24 |   938.3 ms | 103.60 ms | 305.48 ms |   835.3 ms |
| NUnit     | 4.4.0   | 1,483.9 ms | 136.52 ms | 398.25 ms | 1,350.8 ms |
| xUnit     | 2.9.3   | 1,186.3 ms |  62.25 ms | 178.61 ms | 1,173.0 ms |
| MSTest    | 3.10.4  | 1,158.7 ms |  97.35 ms | 283.98 ms | 1,075.4 ms |



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    26.55 ms |  0.105 ms |  0.088 ms |    26.54 ms |
| TUnit     | 0.57.24 |   929.99 ms | 18.531 ms | 18.200 ms |   927.09 ms |
| NUnit     | 4.4.0   | 1,303.19 ms |  7.883 ms |  7.373 ms | 1,303.88 ms |
| xUnit     | 2.9.3   | 1,412.59 ms | 11.545 ms | 10.799 ms | 1,412.37 ms |
| MSTest    | 3.10.4  | 1,272.29 ms | 13.102 ms | 12.255 ms | 1,269.89 ms |



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    62.59 ms |  0.659 ms |  0.551 ms |    62.38 ms |
| TUnit     | 0.57.24 | 1,011.45 ms | 20.122 ms | 25.448 ms | 1,011.42 ms |
| NUnit     | 4.4.0   | 1,409.92 ms | 26.702 ms | 24.977 ms | 1,404.02 ms |
| xUnit     | 2.9.3   | 1,504.77 ms | 15.311 ms | 14.322 ms | 1,504.41 ms |
| MSTest    | 3.10.4  | 1,343.68 ms | 14.882 ms | 13.192 ms | 1,344.60 ms |


### Scenario: Simple tests with basic operations and assertions

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean       | Error    | StdDev    | Median     |
|---------- |-------- |-----------:|---------:|----------:|-----------:|
| TUnit_AOT | 0.57.24 |   162.4 ms | 17.72 ms |  51.97 ms |   144.4 ms |
| TUnit     | 0.57.24 |   964.8 ms | 96.96 ms | 284.37 ms |   869.0 ms |
| NUnit     | 4.4.0   | 1,180.6 ms | 69.37 ms | 203.45 ms | 1,164.4 ms |
| xUnit     | 2.9.3   | 1,246.6 ms | 79.95 ms | 230.69 ms | 1,256.0 ms |
| MSTest    | 3.10.4  | 1,082.7 ms | 57.15 ms | 166.70 ms | 1,084.4 ms |



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    26.83 ms |  0.260 ms |  0.218 ms |    26.87 ms |
| TUnit     | 0.57.24 |   965.67 ms | 18.974 ms | 24.672 ms |   959.08 ms |
| NUnit     | 4.4.0   | 1,338.27 ms | 14.512 ms | 13.574 ms | 1,336.51 ms |
| xUnit     | 2.9.3   | 1,414.08 ms | 16.642 ms | 15.567 ms | 1,412.72 ms |
| MSTest    | 3.10.4  | 1,281.62 ms | 14.716 ms | 13.765 ms | 1,280.02 ms |



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    56.30 ms |  1.106 ms |  1.587 ms |    56.33 ms |
| TUnit     | 0.57.24 |   991.06 ms | 19.785 ms | 27.736 ms |   989.46 ms |
| NUnit     | 4.4.0   | 1,329.07 ms | 14.937 ms | 13.972 ms | 1,329.02 ms |
| xUnit     | 2.9.3   | 1,391.78 ms |  8.414 ms |  7.026 ms | 1,390.53 ms |
| MSTest    | 3.10.4  | 1,283.13 ms | 11.959 ms | 11.186 ms | 1,279.70 ms |


### Scenario: Parameterized tests with multiple test cases using data attributes

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean    | Error    | StdDev   | Median  |
|---------- |-------- |--------:|---------:|---------:|--------:|
| TUnit_AOT | 0.57.24 |      NA |       NA |       NA |      NA |
| TUnit     | 0.57.24 |      NA |       NA |       NA |      NA |
| NUnit     | 4.4.0   | 1.566 s | 0.1462 s | 0.4241 s | 1.461 s |
| xUnit     | 2.9.3   | 1.448 s | 0.0743 s | 0.2189 s | 1.396 s |
| MSTest    | 3.10.4  | 1.167 s | 0.0564 s | 0.1592 s | 1.155 s |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean    | Error    | StdDev   | Median  |
|---------- |-------- |--------:|---------:|---------:|--------:|
| TUnit_AOT | 0.57.24 |      NA |       NA |       NA |      NA |
| TUnit     | 0.57.24 |      NA |       NA |       NA |      NA |
| NUnit     | 4.4.0   | 1.329 s | 0.0203 s | 0.0190 s | 1.336 s |
| xUnit     | 2.9.3   | 1.382 s | 0.0116 s | 0.0109 s | 1.381 s |
| MSTest    | 3.10.4  | 1.282 s | 0.0172 s | 0.0161 s | 1.283 s |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean    | Error    | StdDev   | Median  |
|---------- |-------- |--------:|---------:|---------:|--------:|
| TUnit_AOT | 0.57.24 |      NA |       NA |       NA |      NA |
| TUnit     | 0.57.24 |      NA |       NA |       NA |      NA |
| NUnit     | 4.4.0   | 1.337 s | 0.0084 s | 0.0075 s | 1.337 s |
| xUnit     | 2.9.3   | 1.391 s | 0.0136 s | 0.0128 s | 1.387 s |
| MSTest    | 3.10.4  | 1.281 s | 0.0100 s | 0.0084 s | 1.280 s |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)


### Scenario: Tests utilizing class fixtures and shared test context

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean       | Error     | StdDev    | Median     |
|---------- |-------- |-----------:|----------:|----------:|-----------:|
| TUnit_AOT | 0.57.24 |   202.8 ms |  14.73 ms |  43.42 ms |   205.8 ms |
| TUnit     | 0.57.24 | 1,182.5 ms | 109.64 ms | 318.09 ms | 1,094.6 ms |
| NUnit     | 4.4.0   | 1,112.6 ms |  68.64 ms | 194.72 ms | 1,062.7 ms |
| xUnit     | 2.9.3   | 1,153.4 ms |  73.93 ms | 215.66 ms | 1,127.4 ms |
| MSTest    | 3.10.4  |         NA |        NA |        NA |         NA |

Benchmarks with issues:
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    25.27 ms |  0.059 ms |  0.049 ms |    25.27 ms |
| TUnit     | 0.57.24 |   933.10 ms | 17.673 ms | 21.039 ms |   936.02 ms |
| NUnit     | 4.4.0   | 1,271.72 ms | 10.168 ms |  9.511 ms | 1,273.77 ms |
| xUnit     | 2.9.3   | 1,346.72 ms |  8.693 ms |  8.131 ms | 1,345.79 ms |
| MSTest    | 3.10.4  |          NA |        NA |        NA |          NA |

Benchmarks with issues:
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    54.56 ms |  1.018 ms |  1.615 ms |    53.93 ms |
| TUnit     | 0.57.24 | 1,015.53 ms | 20.206 ms | 25.554 ms | 1,005.45 ms |
| NUnit     | 4.4.0   | 1,362.98 ms | 18.645 ms | 17.440 ms | 1,365.75 ms |
| xUnit     | 2.9.3   | 1,426.93 ms | 19.671 ms | 18.400 ms | 1,426.68 ms |
| MSTest    | 3.10.4  |          NA |        NA |        NA |          NA |

Benchmarks with issues:
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)


### Scenario: Tests executing in parallel to test framework parallelization

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean       | Error    | StdDev    | Median     |
|---------- |-------- |-----------:|---------:|----------:|-----------:|
| TUnit_AOT | 0.57.24 |   125.8 ms |  6.15 ms |  17.65 ms |   120.8 ms |
| TUnit     | 0.57.24 |   726.6 ms | 50.01 ms | 140.22 ms |   680.1 ms |
| NUnit     | 4.4.0   | 1,061.8 ms | 66.68 ms | 195.57 ms | 1,012.2 ms |
| xUnit     | 2.9.3   |   897.8 ms | 21.82 ms |  63.31 ms |   888.5 ms |
| MSTest    | 3.10.4  |   951.5 ms | 37.15 ms | 108.36 ms |   940.5 ms |



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    27.47 ms |  0.404 ms |  0.377 ms |    27.38 ms |
| TUnit     | 0.57.24 |   970.36 ms | 18.912 ms | 21.021 ms |   971.13 ms |
| NUnit     | 4.4.0   | 1,350.57 ms | 13.405 ms | 11.883 ms | 1,350.61 ms |
| xUnit     | 2.9.3   | 1,424.20 ms | 12.285 ms | 11.491 ms | 1,426.66 ms |
| MSTest    | 3.10.4  | 1,292.03 ms | 11.003 ms | 10.292 ms | 1,291.20 ms |



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    55.16 ms |  1.060 ms |  1.883 ms |    55.25 ms |
| TUnit     | 0.57.24 | 1,007.83 ms | 19.885 ms | 25.148 ms | 1,011.61 ms |
| NUnit     | 4.4.0   | 1,358.32 ms | 14.114 ms | 13.202 ms | 1,355.64 ms |
| xUnit     | 2.9.3   | 1,414.91 ms | 12.122 ms | 10.746 ms | 1,418.93 ms |
| MSTest    | 3.10.4  | 1,296.65 ms | 20.527 ms | 19.201 ms | 1,297.78 ms |


### Scenario: A test that takes 50ms to execute, repeated 100 times

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean | Error | StdDev | Median |
|---------- |-------- |-----:|------:|-------:|-------:|
| TUnit_AOT | 0.57.24 |   NA |    NA |     NA |     NA |
| TUnit     | 0.57.24 |   NA |    NA |     NA |     NA |
| NUnit     | 4.4.0   |   NA |    NA |     NA |     NA |
| xUnit     | 2.9.3   |   NA |    NA |     NA |     NA |
| MSTest    | 3.10.4  |   NA |    NA |     NA |     NA |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.NUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.xUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean | Error | StdDev | Median |
|---------- |-------- |-----:|------:|-------:|-------:|
| TUnit_AOT | 0.57.24 |   NA |    NA |     NA |     NA |
| TUnit     | 0.57.24 |   NA |    NA |     NA |     NA |
| NUnit     | 4.4.0   |   NA |    NA |     NA |     NA |
| xUnit     | 2.9.3   |   NA |    NA |     NA |     NA |
| MSTest    | 3.10.4  |   NA |    NA |     NA |     NA |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.NUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.xUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean | Error | StdDev | Median |
|---------- |-------- |-----:|------:|-------:|-------:|
| TUnit_AOT | 0.57.24 |   NA |    NA |     NA |     NA |
| TUnit     | 0.57.24 |   NA |    NA |     NA |     NA |
| NUnit     | 4.4.0   |   NA |    NA |     NA |     NA |
| xUnit     | 2.9.3   |   NA |    NA |     NA |     NA |
| MSTest    | 3.10.4  |   NA |    NA |     NA |     NA |

Benchmarks with issues:
  RuntimeBenchmarks.TUnit_AOT: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.TUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.NUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.xUnit: Job-YNJDZW(Runtime=.NET 9.0)
  RuntimeBenchmarks.MSTest: Job-YNJDZW(Runtime=.NET 9.0)


### Scenario: Tests with setup and teardown lifecycle methods

#### macos-latest

```

BenchmarkDotNet v0.15.2, macOS Sequoia 15.5 (24F74) [Darwin 24.5.0]
Apple M1 (Virtual), 1 CPU, 3 logical and 3 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), Arm64 RyuJIT AdvSIMD

Runtime=.NET 9.0  

```
| Method    | Version | Mean       | Error     | StdDev    | Median     |
|---------- |-------- |-----------:|----------:|----------:|-----------:|
| TUnit_AOT | 0.57.24 |   185.1 ms |  18.67 ms |  54.74 ms |   181.9 ms |
| TUnit     | 0.57.24 |   895.4 ms | 101.98 ms | 290.96 ms |   787.7 ms |
| NUnit     | 4.4.0   | 1,316.6 ms | 105.67 ms | 304.88 ms | 1,256.3 ms |
| xUnit     | 2.9.3   | 1,189.4 ms |  64.20 ms | 186.26 ms | 1,168.3 ms |
| MSTest    | 3.10.4  | 1,171.6 ms |  86.32 ms | 247.67 ms | 1,119.3 ms |



#### ubuntu-latest

```

BenchmarkDotNet v0.15.2, Linux Ubuntu 24.04.3 LTS (Noble Numbat)
AMD EPYC 7763, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    26.84 ms |  0.421 ms |  0.373 ms |    26.72 ms |
| TUnit     | 0.57.24 |   953.13 ms | 18.458 ms | 19.750 ms |   949.42 ms |
| NUnit     | 4.4.0   | 1,315.44 ms |  9.953 ms |  8.823 ms | 1,317.09 ms |
| xUnit     | 2.9.3   | 1,388.58 ms | 12.037 ms | 11.259 ms | 1,387.18 ms |
| MSTest    | 3.10.4  | 1,272.21 ms | 11.161 ms | 10.440 ms | 1,271.47 ms |



#### windows-latest

```

BenchmarkDotNet v0.15.2, Windows 10 (10.0.20348.4052) (Hyper-V)
AMD EPYC 7763 2.44GHz, 1 CPU, 4 logical and 2 physical cores
.NET SDK 9.0.304
  [Host]     : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2
  Job-YNJDZW : .NET 9.0.8 (9.0.825.36511), X64 RyuJIT AVX2

Runtime=.NET 9.0  

```
| Method    | Version | Mean        | Error     | StdDev    | Median      |
|---------- |-------- |------------:|----------:|----------:|------------:|
| TUnit_AOT | 0.57.24 |    49.07 ms |  0.969 ms |  0.809 ms |    49.41 ms |
| TUnit     | 0.57.24 | 1,000.09 ms | 19.691 ms | 23.441 ms |   997.06 ms |
| NUnit     | 4.4.0   | 1,338.53 ms | 15.538 ms | 14.534 ms | 1,341.42 ms |
| xUnit     | 2.9.3   | 1,382.45 ms | 14.360 ms | 11.991 ms | 1,379.46 ms |
| MSTest    | 3.10.4  | 1,291.51 ms | 21.662 ms | 20.263 ms | 1,285.32 ms |



