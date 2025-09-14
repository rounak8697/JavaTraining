# Unit Testing Workflow Guide for TxBusinessRest Project

## 🎯 Overview

This project uses **xUnit** as the testing framework, **Moq** for mocking, and **AutoFixture** for test data generation. The testing structure follows a **layered approach** with separate test projects for different layers.

## 🏗️ Test Project Structure

```
TxBusinessRest/
├── TxModelTests/              # Data Layer Tests
│   ├── Repositories/          # Repository tests
│   ├── Fixtures/             # Database fixtures
│   └── Utilities/            # Test utilities
├── TxBusinessUnitTests/       # Business Layer Tests
│   ├── Piloters/             # Service tests
│   └── Services/             # Business logic tests
├── TxObjectTests/            # Object Management Tests
├── TxAdvancedSearchTests/    # Search Functionality Tests
└── TxHourTrackingTests/      # Time Tracking Tests
```

## 🧪 Testing Framework Stack

| Component | Purpose | Java Equivalent |
|-----------|---------|-----------------|
| **xUnit** | Test framework | JUnit 5 |
| **Moq** | Mocking framework | Mockito |
| **AutoFixture** | Test data generation | Faker/JavaFaker |
| **Microsoft.NET.Test.Sdk** | Test runner | Maven Surefire |

## 📋 Unit Testing Workflow

### Step 1: Understanding the Test Structure

#### Base Test Classes
The project uses **inheritance-based testing** with abstract base classes:

```csharp
// Base class for all repository tests
public abstract class IdTests<TConcept, TRepository> : IDisposable
    where TConcept : IIdConcept
    where TRepository : IIdRepository<TConcept>
{
    protected UnitOfWork UnitOfWork { get; private set; }
    protected TRepository Repository { get; private set; }
    protected IPostprocessComposer<TConcept> Composer { get; set; }
    protected IList<TConcept> KnownRecords { get; set; }
    
    // Common test methods
    [Fact] public virtual void GetShouldWork() { }
    [Fact] public virtual void InsertRandomShouldWork() { }
    [Fact] public virtual void UpdateShouldWork() { }
    [Fact] public virtual void DeleteShouldWorkOrNot() { }
}
```

#### Concrete Test Implementation
```csharp
public class DataTests : IdTests<DData, IDataRepository<DData>>
{
    protected override void DoDeleteShouldWorkOrNot(DData obj)
    {
        Assert.True(Repository.Delete(obj.IdAttribute, obj.IdObject));
        Assert.Null(Get(obj));
    }
    
    protected override TConcept Get(TConcept expected)
    {
        return Repository.Get(expected.IdAttribute, expected.IdObject);
    }
}
```

### Step 2: Setting Up Test Data

#### Using AutoFixture
```csharp
protected IdTests()
{
    Fixture fixture = new Fixture();
    
    // Remove recursion behavior
    fixture.Behaviors.OfType<ThrowingRecursionBehavior>()
        .ToList().ForEach(b => fixture.Behaviors.Remove(b));
    fixture.Behaviors.Add(new OmitOnRecursionBehavior());
    
    // Custom configurations
    fixture.Customizations.Add(new NullIntSpecimenBuilder());
    fixture.Customizations.Add(new OADateTimeRoundingSpecimenBuilder());
    
    // Create composer for random objects
    Composer = fixture.Build<TConcept>().Without(x => x.Id);
}
```

#### Creating Test Data
```csharp
protected TConcept GetRandomNew()
{
    TConcept rndNew = Composer.Create(); // Auto-generate with random properties
    InitializeNew(rndNew); // Set required properties
    return rndNew;
}

protected TConcept GetNew()
{
    return Activator.CreateInstance<TConcept>();
}
```

### Step 3: Writing Test Methods

#### Basic Test Structure
```csharp
[Fact] // xUnit attribute (like @Test in JUnit)
public virtual void GetShouldWork()
{
    // Arrange (Given)
    if (KnownRecords.Count < 1) return;
    TConcept expected = KnownRecords.First();
    
    // Act (When)
    Assert.Null(Record.Exception(() => CompareGet(expected)));
    
    // Assert (Then) - handled in CompareGet method
}

protected virtual void CompareGet(TConcept expected)
{
    TConcept actual = Get(expected);
    DoCompareGet(expected, actual);
}
```

#### Test Method Naming Convention
- **MethodName** + **ShouldWork** (e.g., `GetShouldWork`, `InsertRandomShouldWork`)
- **MethodName** + **ShouldWorkOrNot** (e.g., `DeleteShouldWorkOrNot`)

### Step 4: Mocking with Moq

#### Setting Up Mocks
```csharp
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _mockUserRepository;
    private readonly UserService _userService;
    
    public UserServiceTests()
    {
        _mockUserRepository = new Mock<IUserRepository>();
        _userService = new UserService(_mockUserRepository.Object);
    }
}
```

#### Mocking Method Calls
```csharp
[Fact]
public void GetUser_ShouldReturnUser_WhenIdExists()
{
    // Arrange
    var expectedUser = new User { Id = 1, Name = "John" };
    _mockUserRepository.Setup(x => x.Get(1)).Returns(expectedUser);
    
    // Act
    var result = _userService.GetUser(1);
    
    // Assert
    Assert.NotNull(result);
    Assert.Equal("John", result.Name);
    _mockUserRepository.Verify(x => x.Get(1), Times.Once);
}
```

### Step 5: Database Testing

#### Database Fixtures
```csharp
public abstract class AbstractDatabaseFixture : IDatabaseFixture, IDisposable
{
    public AbstractDatabaseFixture()
    {
        Setup();
    }
    
    protected abstract void Setup();
    
    public void Dispose()
    {
        GlobalConfig.Unload();
    }
}
```

#### Test Collections
```csharp
[CollectionDefinition("MsSqlTestCollection")]
public class MsSqlTestCollection : ICollectionFixture<MsSqlTestFixture>
{
    // This class has no code, and is never created. 
    // Its purpose is simply to be the place to apply [CollectionDefinition] and all the ICollectionFixture<> interfaces.
}
```

## 🔄 Complete Testing Workflow

### 1. **Setup Phase**
```csharp
[Collection("MsSqlTestCollection")]
public class DataTests : IdTests<DData, IDataRepository<DData>>
{
    public DataTests(MsSqlTestFixture fixture) : base()
    {
        // Database setup handled by fixture
    }
}
```

### 2. **Test Execution Phase**
```csharp
[Fact]
public virtual void InsertRandomShouldWork()
{
    // Arrange
    TConcept expected = GetRandomNew();
    
    // Act
    DoInsertShouldWork(expected);
    
    // Assert - handled in DoInsertShouldWork
}

private void DoInsertShouldWork(TConcept expected)
{
    Repository.Insert(expected);
    Assert.True(expected.Id > 0);
    
    TConcept actual = Get(expected);
    Compare(expected, actual);
}
```

### 3. **Cleanup Phase**
```csharp
public void Dispose()
{
    UnitOfWork.Dispose(); // Cleanup database connections
}
```

## 🛠️ Running Tests

### Command Line
```bash
# Run all tests
dotnet test

# Run specific test project
dotnet test TxModelTests

# Run with coverage
dotnet test --collect:"XPlat Code Coverage"

# Run specific test class
dotnet test --filter "ClassName=DataTests"
```

### Visual Studio
1. **Test Explorer** - View all tests
2. **Run All Tests** - Execute entire test suite
3. **Debug Tests** - Step through test execution
4. **Code Coverage** - View coverage reports

## 📊 Test Categories and Patterns

### 1. **Repository Tests** (Data Layer)
- **CRUD Operations**: Insert, Get, Update, Delete
- **Query Operations**: GetAll, GetMultiple, Count
- **Data Validation**: Type checking, constraint validation

### 2. **Service Tests** (Business Layer)
- **Business Logic**: Complex operations, validations
- **Integration**: Service-to-service communication
- **Error Handling**: Exception scenarios

### 3. **Controller Tests** (API Layer)
- **HTTP Methods**: GET, POST, PUT, DELETE
- **Request/Response**: DTO mapping, status codes
- **Authentication**: Authorization, security

## 🎯 Best Practices

### 1. **Test Naming**
```csharp
// Good
[Fact] public void GetUser_ShouldReturnUser_WhenIdExists() { }
[Fact] public void GetUser_ShouldReturnNull_WhenIdDoesNotExist() { }

// Avoid
[Fact] public void Test1() { }
[Fact] public void GetUser() { }
```

### 2. **Arrange-Act-Assert Pattern**
```csharp
[Fact]
public void CalculateTotal_ShouldReturnCorrectSum_WhenItemsExist()
{
    // Arrange
    var items = new List<Item> { new Item { Price = 10 }, new Item { Price = 20 } };
    var calculator = new PriceCalculator();
    
    // Act
    var result = calculator.CalculateTotal(items);
    
    // Assert
    Assert.Equal(30, result);
}
```

### 3. **One Assert Per Test**
```csharp
// Good - Single responsibility
[Fact] public void GetUser_ShouldReturnUser_WhenIdExists() { }
[Fact] public void GetUser_ShouldReturnNull_WhenIdDoesNotExist() { }

// Avoid - Multiple responsibilities
[Fact] public void GetUser_ShouldHandleAllScenarios() { }
```

### 4. **Test Data Management**
```csharp
// Use AutoFixture for random data
var user = _fixture.Create<User>();

// Use builders for specific scenarios
var adminUser = _fixture.Build<User>()
    .With(x => x.Role, "Admin")
    .Create();
```

## 🚨 Common Pitfalls and Solutions

### 1. **Database State Issues**
```csharp
// Problem: Tests affecting each other
[Fact] public void Test1() { /* inserts data */ }
[Fact] public void Test2() { /* expects clean state */ }

// Solution: Use transactions or cleanup
[Fact] public void Test1() { 
    using var transaction = BeginTransaction();
    // test code
    transaction.Rollback();
}
```

### 2. **Mock Verification**
```csharp
// Problem: Not verifying mock calls
_mockRepository.Setup(x => x.Get(1)).Returns(user);
var result = service.GetUser(1);
// Missing verification

// Solution: Verify interactions
_mockRepository.Verify(x => x.Get(1), Times.Once);
_mockRepository.Verify(x => x.Save(It.IsAny<User>()), Times.Never);
```

### 3. **Async Testing**
```csharp
// Problem: Not awaiting async methods
[Fact] public void TestAsync() {
    var result = service.GetUserAsync(1); // Missing await
}

// Solution: Use async/await
[Fact] public async Task TestAsync() {
    var result = await service.GetUserAsync(1);
    Assert.NotNull(result);
}
```

## 📈 Test Coverage and Quality

### Coverage Reports
```bash
# Generate coverage report
dotnet test --collect:"XPlat Code Coverage" --results-directory ./TestResults

# View coverage (requires ReportGenerator)
dotnet tool install -g dotnet-reportgenerator-globaltool
reportgenerator -reports:"./TestResults/**/coverage.cobertura.xml" -targetdir:"./CoverageReport" -reporttypes:Html
```

### Quality Metrics
- **Line Coverage**: Aim for >80%
- **Branch Coverage**: Aim for >70%
- **Test Count**: At least 1 test per public method
- **Test Speed**: Keep individual tests under 100ms

## 🔧 Debugging Tests

### 1. **Debug Mode**
```csharp
[Fact]
public void DebugTest()
{
    // Set breakpoints here
    var result = service.GetUser(1);
    Assert.NotNull(result);
}
```

### 2. **Test Output**
```csharp
[Fact]
public void TestWithOutput()
{
    var result = service.GetUser(1);
    
    // Output for debugging
    _output.WriteLine($"Result: {result}");
    
    Assert.NotNull(result);
}
```

### 3. **Conditional Testing**
```csharp
[Fact]
public void TestWithCondition()
{
    if (Environment.GetEnvironmentVariable("RUN_INTEGRATION_TESTS") == "true")
    {
        // Integration test code
    }
    else
    {
        // Skip or mock
    }
}
```

## 🚀 Next Steps

1. **Start with Repository Tests** - Easiest to understand
2. **Move to Service Tests** - Business logic validation
3. **Add Controller Tests** - API endpoint testing
4. **Implement Integration Tests** - End-to-end scenarios
5. **Set up CI/CD** - Automated testing pipeline

---

*This workflow guide is specifically designed for the TxBusinessRest project. Follow these patterns and practices for consistent, maintainable tests.*
