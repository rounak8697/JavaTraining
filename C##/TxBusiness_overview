# TEEXMA Business REST API - Project Understanding Guide

## Table of Contents
- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Java to C# Quick Reference](#java-to-c-quick-reference)
- [Key Technologies](#key-technologies)
- [Application Workflow](#application-workflow)
- [Unit Testing Strategy](#unit-testing-strategy)
- [2-Day Learning Plan](#2-day-learning-plan)
- [Essential Tools & Commands](#essential-tools--commands)
- [Testing Patterns](#testing-patterns)
- [Quick Start Guide](#quick-start-guide)
- [Pro Tips for Java Developers](#pro-tips-for-java-developers)

## Project Overview

**TEEXMA Business REST API** is a .NET 8 web API that exposes TEEXMA's database metamodel alongside its business intelligence. It's designed to allow programmatic interaction with the TEEXMA ecosystem.

### Business Domain
- **TEEXMA**: A database metamodel management system
- **Core Models**: Objects with Core Model Tags (CMT) for export/import
- **Business Intelligence**: Data analysis and reporting capabilities
- **User Management**: Authentication, authorization, and role-based access

## Architecture

```
📁 Project Structure:
├── TxBusinessRest/          # 🌐 Web API Layer (Controllers, DTOs, Filters)
├── TxBusiness/              # 💼 Business Logic Layer (Services, Models, Piloters)
├── TxModel/                 # 🗄️ Data Access Layer (Repositories, UnitOfWork)
├── TxConfiguration/         # ⚙️ Configuration Management
├── TxObject/               # 📦 Object Management
└── TxBusinessUnitTests/    # 🧪 Unit Tests
```

### Layer Responsibilities

| Layer | Purpose | Key Components |
|-------|---------|----------------|
| **TxBusinessRest** | Web API | Controllers, DTOs, Filters, Converters |
| **TxBusiness** | Business Logic | Services, Piloters, Models, Helpers |
| **TxModel** | Data Access | Repositories, UnitOfWork, Models |
| **TxConfiguration** | Configuration | Settings, Configuration Readers |
| **TxObject** | Object Management | Object Services, Helpers |

## Java to C# Quick Reference

| Java Concept | C#/.NET Equivalent | Example |
|--------------|-------------------|---------|
| `@RestController` | `[ApiController]` | `[ApiController] public class MyController` |
| `@Service` | `[InjectedAs(ServiceLifetime.Transient)]` | `[InjectedAs(ServiceLifetime.Transient)] public class MyService` |
| `@Repository` | Repository pattern | `public class MyRepository : IMyRepository` |
| `@Autowired` | Constructor injection | `public MyController(IMyService service)` |
| `@Valid` | `ValidateAndThrow()` | `new MyValidator().ValidateAndThrow(dto)` |
| `@MockBean` | `Mock<T>` | `Mock<IMyService> mockService = new Mock<IMyService>()` |
| `@Test` | `[Fact]` | `[Fact] public void MyTest()` |
| `@BeforeEach` | `[CreateIconsFolders]` | `[CreateIconsFolders] public void MyTest()` |
| `List<String>` | `List<string>` | `List<string> items = new List<string>()` |
| `Optional<String>` | `string?` | `string? optionalValue` |
| `@NotNull` | `!` operator | `string name = value!` |

## Key Technologies

### Core Framework
- **.NET 8**: Latest LTS version
- **ASP.NET Core**: Web API framework
- **Entity Framework**: ORM (not directly visible but used in TxModel)

### Testing Framework
- **xUnit**: Test framework (like JUnit)
- **Moq**: Mocking framework (like Mockito)
- **FluentValidation**: Input validation testing
- **Microsoft.AspNetCore.Mvc.Testing**: Integration testing

### Additional Libraries
- **AutoMapper**: Object-to-object mapping
- **Swashbuckle**: Swagger/OpenAPI documentation
- **NLog**: Logging framework
- **JwtUtils**: JWT token handling

## Application Workflow

### Request Flow
```
HTTP Request → Controller → Piloter → Service → Repository → Database
```

### Response Flow
```
Database → Repository → Service → Piloter → Controller → DTO → JSON Response
```

### Key Components Flow
1. **Controllers**: Handle HTTP requests/responses, validate input, authorize users
2. **Piloters**: Orchestrate business logic, coordinate between services
3. **Services**: Core business operations, data processing
4. **Repositories**: Data access, database operations
5. **DTOs**: Data transfer objects for API serialization

## Unit Testing Strategy

### Test Categories

#### 1. Controller Tests (`FunctionalTests/`)
Integration tests that test the full HTTP request/response cycle.

```csharp
[Fact]
public async Task GetRequest_ShouldReturnEmptyList()
{
    // Arrange
    string? bearerToken = await GenerateBearerTokenAsync();
    TestAPIWebApplication app = new TestAPIWebApplication();
    app._coreModelExportPiloter.Setup(s => s.GetAll()).Returns(coreModelConcepts);
    
    // Act
    HttpClient client = app.CreateClient();
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, "http://localhost/api/coremodel");
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
    HttpResponseMessage response = await client.SendAsync(request);
    
    // Assert
    Assert.Equal(HttpStatusCode.OK, response.StatusCode);
}
```

#### 2. Service/Piloter Tests (`Piloters/`)
Unit tests for business logic components.

```csharp
[Fact]
public void GivenDifferentObjectType_whenAddOrUpdate_thenReturnConflict()
{
    // Arrange
    SAttribute existingAttribute = CreateDefaultAttribute();
    existingAttribute.IdObjectType = 1;
    SAttribute importedAttribute = CreateDefaultAttribute();
    importedAttribute.IdObjectType = 2;
    
    _attributeService.Setup(s => s.Get(It.IsAny<string>(), false)).Returns(existingAttribute);
    SAttributeCoreService sut = new SAttributeCoreService(_attributeService.Object);
    
    // Act
    IList<ImportedConcept> result = sut.AddOrUpdate(importedAttributeList, _defaultIdEquivalence, 1, true);
    
    // Assert
    Assert.True(result[0].Conflicts.Contains(ImportErrorKeys.AttributeChangedObjectType));
}
```

#### 3. Validation Tests (`FluentValidatorTests/`)
Tests for input validation using FluentValidation.

```csharp
[Fact]
public void validDTO_ShouldNotCauseError()
{
    // Arrange
    CoreModelExportInfoDTO exportInfoDTO = new CoreModelExportInfoDTO
    {
        Version = "1.0.0",
        Description = "description",
        Comment = "comment",
        Date = DateTime.Now
    };
    var validator = new CoreModelExportInfoDTOValidator();
    
    // Act
    var result = validator.TestValidate(exportInfoDTO);
    
    // Assert
    Assert.Empty(result.Errors);
}
```

### Testing Patterns

#### AAA Pattern (Arrange, Act, Assert)
```csharp
[Fact]
public void TestMethod()
{
    // Arrange - Set up test data and mocks
    var mockService = new Mock<IMyService>();
    mockService.Setup(s => s.GetData()).Returns(testData);
    
    // Act - Execute the method under test
    var result = sut.MethodUnderTest();
    
    // Assert - Verify the results
    Assert.Equal(expectedValue, result);
}
```

#### Theory Tests for Multiple Cases
```csharp
[Theory]
[InlineData(0, 0, 1, 0, ImportErrorKeys.AttributeChangedLowerBound)]
[InlineData(0, 0, 0, 1, ImportErrorKeys.AttributeChangedUpperBound)]
public void GivenDifferentBounds_whenAddOrUpdate_thenReturnConflict(
    int existingLowerBound, int existingUpperBound, 
    int importedLowerBound, int importedUpperBound, 
    ImportErrorKeys importErrorKey)
{
    // Test implementation
}
```

## 2-Day Learning Plan

### Day 1: Understanding the Project (8 hours)

#### Morning (4 hours)
1. **Read Documentation** (30 min)
   - Study `README.md` for project overview
   - Understand TEEXMA business domain

2. **Explore Architecture** (1.5 hours)
   - Examine `Startup.cs` for dependency injection setup
   - Study `AbstractController.cs` for base controller patterns
   - Review `CoreModelController.cs` for API endpoint examples

3. **Understand Data Flow** (2 hours)
   - Explore DTOs in `TxBusinessRest/DTOs/`
   - Study services in `TxBusiness/Services/`
   - Examine piloters in `TxBusiness/Piloters/`

#### Afternoon (4 hours)
1. **Study Business Logic** (2 hours)
   - Review models in `TxBusiness/Models/`
   - Understand the Core Model concept
   - Study validation patterns

2. **Explore Data Layer** (2 hours)
   - Examine repositories in `TxModel/Repositories/`
   - Understand UnitOfWork pattern
   - Study entity models

### Day 2: Writing Unit Tests (8 hours)

#### Morning (4 hours)
1. **Study Existing Tests** (2 hours)
   - Review test patterns in `TxBusinessUnitTests/`
   - Understand mocking with Moq
   - Learn xUnit test attributes and assertions

2. **Practice Basic Tests** (2 hours)
   - Write simple service tests
   - Practice mocking dependencies
   - Test validation logic

#### Afternoon (4 hours)
1. **Controller Testing** (2 hours)
   - Write controller tests using `TestAPIWebApplication`
   - Test authentication and authorization
   - Test error handling

2. **Integration Testing** (2 hours)
   - Write end-to-end tests
   - Test complex workflows
   - Debug and fix test issues

## Essential Tools & Commands

### Development Commands
```bash
# Build the solution
dotnet build

# Run the API
dotnet run --project TxBusinessRest

# Run all tests
dotnet test

# Run specific test project
dotnet test TxBusinessUnitTests/

# Run tests with coverage
dotnet test --collect:"XPlat Code Coverage"

# Clean and rebuild
dotnet clean && dotnet build
```

### Database Setup (Docker)
```bash
# Start test database
cd TxModelTests/Databases
./StartServer.ps1

# Stop database
./ShutDownServer.ps1

# Backup database
./BackupDatabase.ps1
```

### IDE Setup
- **Visual Studio 2022** or **JetBrains Rider**
- **Postman** or **Insomnia** for API testing
- **SQL Server Management Studio** for database access

## Testing Patterns

### 1. Test Naming Convention
```csharp
// Pattern: GivenX_whenY_thenZ
[Fact]
public void GivenValidInput_whenProcessing_thenReturnsSuccess()
{
    // Test implementation
}
```

### 2. Mock Setup Patterns
```csharp
// Setup method with specific parameters
mockService.Setup(s => s.GetById(It.IsAny<int>())).Returns(testData);

// Setup method with any parameters
mockService.Setup(s => s.GetAll()).Returns(testList);

// Setup method to throw exception
mockService.Setup(s => s.GetById(It.IsAny<int>())).Throws<NotFoundException>();
```

### 3. Assertion Patterns
```csharp
// Basic assertions
Assert.Equal(expected, actual);
Assert.True(condition);
Assert.False(condition);
Assert.Null(value);
Assert.NotNull(value);

// Collection assertions
Assert.Empty(collection);
Assert.NotEmpty(collection);
Assert.Contains(item, collection);
Assert.DoesNotContain(item, collection);

// Exception assertions
Assert.Throws<ExceptionType>(() => methodCall());
```

### 4. Test Data Setup
```csharp
private SAttribute CreateDefaultAttribute()
{
    return new SAttribute()
    {
        IdObjectType = 1,
        DataType = Constants.DataTypeString,
        Name = "TestAttribute"
    };
}
```

## Quick Start Guide

### 1. Setting Up Your First Test
```csharp
public class MyServiceTests
{
    private readonly Mock<IMyRepository> _mockRepository;
    private readonly MyService _sut;

    public MyServiceTests()
    {
        _mockRepository = new Mock<IMyRepository>();
        _sut = new MyService(_mockRepository.Object);
    }

    [Fact]
    public void GivenValidId_whenGetById_thenReturnsEntity()
    {
        // Arrange
        var expectedEntity = new MyEntity { Id = 1, Name = "Test" };
        _mockRepository.Setup(r => r.GetById(1)).Returns(expectedEntity);

        // Act
        var result = _sut.GetById(1);

        // Assert
        Assert.Equal(expectedEntity, result);
    }
}
```

### 2. Controller Test Setup
```csharp
[Fact]
public async Task Get_ShouldReturnOkResult()
{
    // Arrange
    var mockService = new Mock<IMyService>();
    mockService.Setup(s => s.GetAll()).Returns(new List<MyEntity>());
    
    var controller = new MyController(mockService.Object, mapper);
    
    // Act
    var result = controller.Get();
    
    // Assert
    Assert.IsType<OkObjectResult>(result.Result);
}
```

### 3. Running Your Tests
```bash
# Run all tests
dotnet test

# Run specific test class
dotnet test --filter "ClassName=MyServiceTests"

# Run tests with detailed output
dotnet test --verbosity normal
```

## Pro Tips for Java Developers

### 1. Language Differences
- **Nullable Reference Types**: C# has nullable reference types (`string?`), be aware of null checks
- **Property Syntax**: C# uses properties (`{ get; set; }`) instead of getters/setters
- **LINQ**: Similar to Java Streams, but more powerful and integrated
- **Async/Await**: Similar to Java's CompletableFuture, but more integrated

### 2. Framework Differences
- **Dependency Injection**: Constructor injection is preferred over field injection
- **Configuration**: Uses `appsettings.json` instead of `application.properties`
- **Logging**: Built-in logging with `ILogger<T>` instead of SLF4J
- **Validation**: FluentValidation instead of Bean Validation

### 3. Testing Differences
- **Mocking**: Moq uses `Setup()` and `Verify()` instead of `when()` and `verify()`
- **Assertions**: xUnit uses `Assert.Equal()` instead of `assertEquals()`
- **Test Attributes**: `[Fact]` instead of `@Test`, `[Theory]` instead of `@ParameterizedTest`

### 4. Common Pitfalls
- **Null Reference Exceptions**: Be careful with nullable types
- **Async/Await**: Don't forget `await` keywords
- **Disposal**: Use `using` statements for disposable resources
- **String Interpolation**: Use `$"Hello {name}"` instead of `"Hello " + name`

## Conclusion

This TEEXMA Business REST API project follows enterprise patterns similar to Spring Boot applications, making it familiar for Java developers. The key is to:

1. **Understand the business domain** (TEEXMA, Core Models)
2. **Learn the layered architecture** (Controllers → Piloters → Services → Repositories)
3. **Master the testing patterns** (xUnit, Moq, FluentValidation)
4. **Practice with existing examples** before writing new tests

Focus on understanding the TEEXMA business domain first, then the technical implementation will make more sense. The project is well-structured and follows industry best practices, making it an excellent learning opportunity for transitioning from Java to C#/.NET.
