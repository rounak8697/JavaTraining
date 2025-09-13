# Complete C# Learning Roadmap with Unit Testing Focus

## Phase 1: C# Fundamentals (Weeks 1-4)

### Week 1: Getting Started & Basic Syntax

**Key Topics:**
- Setting up .NET development environment
- Basic syntax, variables, and data types
- Console input/output
- Comments and code organization

**Core Concepts:**
```csharp
// Basic variable declarations
int age = 25;
string name = "John";
double salary = 50000.50;
bool isActive = true;

// Console operations
Console.WriteLine($"Hello, {name}! You are {age} years old.");
string input = Console.ReadLine();
```

**Practice Tasks:**
1. Create a simple calculator that adds two numbers
2. Build a program that converts temperature (C to F)
3. Make a basic user profile program

**Resources:**
- Microsoft C# Documentation: https://docs.microsoft.com/en-us/dotnet/csharp/
- C# Yellow Book (free): https://www.robmiles.com/c-yellow-book/

### Week 2: Control Flow & Methods

**Key Topics:**
- Conditional statements (if/else, switch)
- Loops (for, while, foreach)
- Method creation and parameters
- Method overloading

**Core Concepts:**
```csharp
// Control flow example
public static string GetGrade(int score)
{
    return score switch
    {
        >= 90 => "A",
        >= 80 => "B",
        >= 70 => "C",
        >= 60 => "D",
        _ => "F"
    };
}

// Method with multiple parameters
public static double CalculateArea(double length, double width)
{
    return length * width;
}
```

**Practice Tasks:**
1. Create a guessing game with loops
2. Build a grade calculator with multiple methods
3. Implement a simple menu system

### Week 3: Arrays & Collections

**Key Topics:**
- Arrays (single and multi-dimensional)
- Lists, Dictionaries, HashSets
- Collection manipulation
- Iteration patterns

**Core Concepts:**
```csharp
// Working with collections
List<string> names = new List<string> { "Alice", "Bob", "Charlie" };
Dictionary<string, int> ages = new Dictionary<string, int>
{
    ["Alice"] = 30,
    ["Bob"] = 25,
    ["Charlie"] = 35
};

// Iteration
foreach (var kvp in ages)
{
    Console.WriteLine($"{kvp.Key} is {kvp.Value} years old");
}
```

**Practice Tasks:**
1. Create a student management system using Lists
2. Build a simple inventory tracker with Dictionary
3. Implement a word frequency counter

### Week 4: Exception Handling & File I/O

**Key Topics:**
- Try-catch-finally blocks
- Custom exceptions
- File reading/writing
- Working with paths

**Core Concepts:**
```csharp
public static string ReadFileContent(string filePath)
{
    try
    {
        return File.ReadAllText(filePath);
    }
    catch (FileNotFoundException)
    {
        throw new ArgumentException($"File not found: {filePath}");
    }
    catch (UnauthorizedAccessException)
    {
        throw new InvalidOperationException("Access denied to file");
    }
}
```

**Practice Tasks:**
1. Create a log file writer with error handling
2. Build a configuration file reader
3. Implement a simple text file database

## Phase 2: Object-Oriented Programming (Weeks 5-8)

### Week 5: Classes & Objects

**Key Topics:**
- Class definition and instantiation
- Properties and fields
- Constructors and method overloading
- Access modifiers

**Core Concepts:**
```csharp
public class BankAccount
{
    private decimal _balance;
    
    public string AccountNumber { get; private set; }
    public decimal Balance => _balance;
    
    public BankAccount(string accountNumber, decimal initialBalance)
    {
        AccountNumber = accountNumber;
        _balance = initialBalance;
    }
    
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("Amount must be positive");
        _balance += amount;
    }
}
```

**Practice Tasks:**
1. Create a Car class with properties and methods
2. Build a simple library book system
3. Implement a basic employee management class

### Week 6: Inheritance & Polymorphism

**Key Topics:**
- Base classes and derived classes
- Method overriding (virtual/override)
- Abstract classes and methods
- Polymorphism concepts

**Core Concepts:**
```csharp
public abstract class Animal
{
    public string Name { get; protected set; }
    
    protected Animal(string name) => Name = name;
    
    public abstract void MakeSound();
    public virtual void Move() => Console.WriteLine($"{Name} is moving");
}

public class Dog : Animal
{
    public Dog(string name) : base(name) { }
    
    public override void MakeSound() => Console.WriteLine($"{Name} barks: Woof!");
    public override void Move() => Console.WriteLine($"{Name} runs on four legs");
}
```

**Practice Tasks:**
1. Create a shape hierarchy (Rectangle, Circle, Triangle)
2. Build a vehicle inheritance system
3. Implement a game character class hierarchy

### Week 7: Interfaces & Composition

**Key Topics:**
- Interface definition and implementation
- Multiple interface implementation
- Composition over inheritance
- Dependency inversion principle

**Core Concepts:**
```csharp
public interface IPaymentProcessor
{
    Task<bool> ProcessPayment(decimal amount, string paymentMethod);
}

public interface INotificationService
{
    Task SendNotification(string message, string recipient);
}

public class OrderService
{
    private readonly IPaymentProcessor _paymentProcessor;
    private readonly INotificationService _notificationService;
    
    public OrderService(IPaymentProcessor paymentProcessor, 
                       INotificationService notificationService)
    {
        _paymentProcessor = paymentProcessor;
        _notificationService = notificationService;
    }
    
    public async Task<bool> ProcessOrder(Order order)
    {
        var success = await _paymentProcessor.ProcessPayment(order.Total, order.PaymentMethod);
        if (success)
        {
            await _notificationService.SendNotification("Order processed", order.CustomerEmail);
        }
        return success;
    }
}
```

**Practice Tasks:**
1. Create a logging system with multiple implementations
2. Build a data access layer with interfaces
3. Implement a plugin-style architecture

### Week 8: Advanced OOP Concepts

**Key Topics:**
- Static classes and members
- Extension methods
- Partial classes
- Nested classes and enums

**Core Concepts:**
```csharp
public static class StringExtensions
{
    public static bool IsValidEmail(this string email)
    {
        return !string.IsNullOrWhiteSpace(email) && 
               email.Contains("@") && 
               email.Contains(".");
    }
}

// Usage
string email = "user@example.com";
if (email.IsValidEmail())
{
    Console.WriteLine("Valid email");
}
```

**Practice Tasks:**
1. Create utility extension methods for common operations
2. Build a configuration system with static helpers
3. Implement a simple state machine using enums

## Phase 3: Introduction to Unit Testing (Weeks 9-10)

### Week 9: Testing Fundamentals & Your First Tests

**Key Topics:**
- What is unit testing and why it matters
- Setting up xUnit test project
- Writing your first tests
- Arrange-Act-Assert pattern
- Test naming conventions

**Core Concepts:**
```csharp
// Class to test
public class Calculator
{
    public int Add(int a, int b) => a + b;
    public int Divide(int a, int b)
    {
        if (b == 0) throw new DivideByZeroException();
        return a / b;
    }
}

// Test class
public class CalculatorTests
{
    [Fact]
    public void Add_TwoPositiveNumbers_ReturnsCorrectSum()
    {
        // Arrange
        var calculator = new Calculator();
        
        // Act
        var result = calculator.Add(5, 3);
        
        // Assert
        Assert.Equal(8, result);
    }
    
    [Fact]
    public void Divide_ByZero_ThrowsDivideByZeroException()
    {
        // Arrange
        var calculator = new Calculator();
        
        // Act & Assert
        Assert.Throws<DivideByZeroException>(() => calculator.Divide(10, 0));
    }
}
```

**Practice Tasks:**
1. Create tests for your BankAccount class from Week 5
2. Write tests for string manipulation methods
3. Test exception scenarios in your file I/O code

### Week 10: Test Organization & Parameterized Tests

**Key Topics:**
- Test class organization
- Setup and teardown (Constructor/Dispose)
- Parameterized tests (Theory/InlineData)
- Testing different scenarios

**Core Concepts:**
```csharp
public class BankAccountTests : IDisposable
{
    private readonly BankAccount _account;
    
    public BankAccountTests()
    {
        _account = new BankAccount("12345", 1000m);
    }
    
    [Theory]
    [InlineData(100, 1100)]
    [InlineData(500, 1500)]
    [InlineData(0.01, 1000.01)]
    public void Deposit_ValidAmount_UpdatesBalance(decimal amount, decimal expectedBalance)
    {
        // Act
        _account.Deposit(amount);
        
        // Assert
        Assert.Equal(expectedBalance, _account.Balance);
    }
    
    [Theory]
    [InlineData(-100)]
    [InlineData(0)]
    public void Deposit_InvalidAmount_ThrowsArgumentException(decimal amount)
    {
        Assert.Throws<ArgumentException>(() => _account.Deposit(amount));
    }
    
    public void Dispose()
    {
        // Cleanup if needed
    }
}
```

**Practice Tasks:**
1. Refactor existing tests to use parameterized tests
2. Create comprehensive tests for your shape hierarchy
3. Test edge cases and boundary conditions

## Phase 4: LINQ & Functional Programming (Week 11)

### Week 11: LINQ & Functional Concepts

**Key Topics:**
- LINQ query syntax and method syntax
- Common LINQ operations (Where, Select, GroupBy, Join)
- Func and Action delegates
- Lambda expressions

**Core Concepts:**
```csharp
public class ProductService
{
    private readonly List<Product> _products;
    
    public ProductService(List<Product> products)
    {
        _products = products;
    }
    
    public IEnumerable<Product> GetExpensiveProducts(decimal minPrice)
    {
        return _products.Where(p => p.Price >= minPrice);
    }
    
    public IEnumerable<ProductSummary> GetProductSummaries()
    {
        return _products.Select(p => new ProductSummary
        {
            Name = p.Name,
            Price = p.Price,
            Category = p.Category
        });
    }
    
    public Dictionary<string, int> GetProductCountByCategory()
    {
        return _products.GroupBy(p => p.Category)
                       .ToDictionary(g => g.Key, g => g.Count());
    }
}

// Tests for LINQ operations
[Fact]
public void GetExpensiveProducts_WithMinPrice100_ReturnsCorrectProducts()
{
    // Arrange
    var products = new List<Product>
    {
        new Product { Name = "Cheap", Price = 50 },
        new Product { Name = "Expensive", Price = 150 }
    };
    var service = new ProductService(products);
    
    // Act
    var result = service.GetExpensiveProducts(100m);
    
    // Assert
    Assert.Single(result);
    Assert.Equal("Expensive", result.First().Name);
}
```

**Practice Tasks:**
1. Create a data analysis class using LINQ
2. Build a search/filter system for your collections
3. Write comprehensive tests for LINQ operations

## Phase 5: Async Programming (Week 12)

### Week 12: Async/Await & Testing Async Code

**Key Topics:**
- Understanding async/await
- Task and Task<T>
- Async best practices
- Testing asynchronous code

**Core Concepts:**
```csharp
public interface IDataRepository
{
    Task<User> GetUserAsync(int id);
    Task SaveUserAsync(User user);
}

public class UserService
{
    private readonly IDataRepository _repository;
    
    public UserService(IDataRepository repository)
    {
        _repository = repository;
    }
    
    public async Task<UserDto> GetUserProfileAsync(int userId)
    {
        var user = await _repository.GetUserAsync(userId);
        if (user == null)
            throw new UserNotFoundException($"User {userId} not found");
            
        return new UserDto
        {
            Id = user.Id,
            Name = user.Name,
            Email = user.Email
        };
    }
}

// Testing async code
[Fact]
public async Task GetUserProfileAsync_ExistingUser_ReturnsUserDto()
{
    // Arrange
    var user = new User { Id = 1, Name = "John", Email = "john@test.com" };
    var mockRepository = new Mock<IDataRepository>();
    mockRepository.Setup(r => r.GetUserAsync(1))
              .ReturnsAsync(user);
    
    var service = new UserService(mockRepository.Object);
    
    // Act
    var result = await service.GetUserProfileAsync(1);
    
    // Assert
    Assert.Equal(1, result.Id);
    Assert.Equal("John", result.Name);
    Assert.Equal("john@test.com", result.Email);
}
```

**Practice Tasks:**
1. Convert synchronous file operations to async
2. Create an async web service client
3. Write tests for all async scenarios including timeouts

## Phase 6: Advanced Testing Concepts (Weeks 13-15)

### Week 13: Mocking with Moq Framework

**Key Topics:**
- Introduction to test doubles (mocks, stubs, fakes)
- Installing and using Moq
- Setting up mock behaviors
- Verifying method calls

**Core Concepts:**
```csharp
public class OrderProcessor
{
    private readonly IPaymentService _paymentService;
    private readonly IInventoryService _inventoryService;
    private readonly IEmailService _emailService;
    
    public OrderProcessor(IPaymentService paymentService, 
                         IInventoryService inventoryService,
                         IEmailService emailService)
    {
        _paymentService = paymentService;
        _inventoryService = inventoryService;
        _emailService = emailService;
    }
    
    public async Task<OrderResult> ProcessOrderAsync(Order order)
    {
        // Check inventory
        var inStock = await _inventoryService.CheckAvailabilityAsync(order.ProductId, order.Quantity);
        if (!inStock)
            return new OrderResult { Success = false, Message = "Out of stock" };
        
        // Process payment
        var paymentResult = await _paymentService.ChargeAsync(order.Amount);
        if (!paymentResult.Success)
            return new OrderResult { Success = false, Message = "Payment failed" };
        
        // Update inventory
        await _inventoryService.ReserveAsync(order.ProductId, order.Quantity);
        
        // Send confirmation email
        await _emailService.SendOrderConfirmationAsync(order.CustomerEmail, order.Id);
        
        return new OrderResult { Success = true, Message = "Order processed successfully" };
    }
}

// Advanced mocking tests
public class OrderProcessorTests
{
    private readonly Mock<IPaymentService> _mockPaymentService;
    private readonly Mock<IInventoryService> _mockInventoryService;
    private readonly Mock<IEmailService> _mockEmailService;
    private readonly OrderProcessor _orderProcessor;
    
    public OrderProcessorTests()
    {
        _mockPaymentService = new Mock<IPaymentService>();
        _mockInventoryService = new Mock<IInventoryService>();
        _mockEmailService = new Mock<IEmailService>();
        
        _orderProcessor = new OrderProcessor(
            _mockPaymentService.Object,
            _mockInventoryService.Object,
            _mockEmailService.Object);
    }
    
    [Fact]
    public async Task ProcessOrderAsync_SuccessfulOrder_ReturnsSuccess()
    {
        // Arrange
        var order = new Order { ProductId = 1, Quantity = 2, Amount = 100m, CustomerEmail = "test@test.com" };
        
        _mockInventoryService.Setup(x => x.CheckAvailabilityAsync(1, 2))
                           .ReturnsAsync(true);
        
        _mockPaymentService.Setup(x => x.ChargeAsync(100m))
                         .ReturnsAsync(new PaymentResult { Success = true });
        
        // Act
        var result = await _orderProcessor.ProcessOrderAsync(order);
        
        // Assert
        Assert.True(result.Success);
        
        // Verify all services were called correctly
        _mockInventoryService.Verify(x => x.CheckAvailabilityAsync(1, 2), Times.Once);
        _mockPaymentService.Verify(x => x.ChargeAsync(100m), Times.Once);
        _mockInventoryService.Verify(x => x.ReserveAsync(1, 2), Times.Once);
        _mockEmailService.Verify(x => x.SendOrderConfirmationAsync("test@test.com", order.Id), Times.Once);
    }
    
    [Fact]
    public async Task ProcessOrderAsync_OutOfStock_DoesNotCharge()
    {
        // Arrange
        var order = new Order { ProductId = 1, Quantity = 2, Amount = 100m };
        
        _mockInventoryService.Setup(x => x.CheckAvailabilityAsync(1, 2))
                           .ReturnsAsync(false);
        
        // Act
        var result = await _orderProcessor.ProcessOrderAsync(order);
        
        // Assert
        Assert.False(result.Success);
        Assert.Equal("Out of stock", result.Message);
        
        // Verify payment was never attempted
        _mockPaymentService.Verify(x => x.ChargeAsync(It.IsAny<decimal>()), Times.Never);
    }
}
```

### Week 14: Advanced Mocking Scenarios

**Key Topics:**
- Callback functions and custom behaviors
- Partial mocks and spy objects
- Mock sequences and complex setups
- Testing with multiple mock interactions

**Core Concepts:**
```csharp
[Fact]
public async Task ProcessOrderAsync_PaymentServiceThrows_HandlesException()
{
    // Arrange
    var order = new Order { ProductId = 1, Quantity = 2, Amount = 100m };
    
    _mockInventoryService.Setup(x => x.CheckAvailabilityAsync(1, 2))
                       .ReturnsAsync(true);
    
    _mockPaymentService.Setup(x => x.ChargeAsync(100m))
                     .ThrowsAsync(new PaymentException("Service unavailable"));
    
    // Act
    var result = await _orderProcessor.ProcessOrderAsync(order);
    
    // Assert
    Assert.False(result.Success);
    Assert.Contains("Payment failed", result.Message);
}

[Fact]
public void DatabaseConnection_CallbackExample()
{
    // Arrange
    var mockConnection = new Mock<IDbConnection>();
    var capturedSql = string.Empty;
    
    mockConnection.Setup(x => x.ExecuteQuery(It.IsAny<string>()))
                .Callback<string>(sql => capturedSql = sql)
                .Returns(new List<User>());
    
    var repository = new UserRepository(mockConnection.Object);
    
    // Act
    repository.GetActiveUsers();
    
    // Assert
    Assert.Contains("WHERE IsActive = 1", capturedSql);
}
```

### Week 15: Testing Strategies & Clean Test Code

**Key Topics:**
- Test organization and naming
- Builder pattern for test data
- Custom assertions and matchers
- Testing private methods (when appropriate)
- Test code maintainability

**Core Concepts:**
```csharp
// Test data builder pattern
public class OrderBuilder
{
    private Order _order = new Order();
    
    public OrderBuilder WithProductId(int productId)
    {
        _order.ProductId = productId;
        return this;
    }
    
    public OrderBuilder WithQuantity(int quantity)
    {
        _order.Quantity = quantity;
        return this;
    }
    
    public OrderBuilder WithAmount(decimal amount)
    {
        _order.Amount = amount;
        return this;
    }
    
    public OrderBuilder WithCustomerEmail(string email)
    {
        _order.CustomerEmail = email;
        return this;
    }
    
    public Order Build() => _order;
    
    public static OrderBuilder CreateDefault() =>
        new OrderBuilder()
            .WithProductId(1)
            .WithQuantity(1)
            .WithAmount(50m)
            .WithCustomerEmail("test@test.com");
}

// Usage in tests
[Fact]
public async Task ProcessOrderAsync_ValidOrder_ProcessesSuccessfully()
{
    // Arrange
    var order = OrderBuilder.CreateDefault()
                          .WithAmount(150m)
                          .WithQuantity(3)
                          .Build();
    
    SetupSuccessfulMocks();
    
    // Act
    var result = await _orderProcessor.ProcessOrderAsync(order);
    
    // Assert
    Assert.True(result.Success);
}

// Custom assertion extensions
public static class CustomAssertions
{
    public static void ShouldBeValidEmail(this string email)
    {
        Assert.True(!string.IsNullOrWhiteSpace(email));
        Assert.Contains("@", email);
        Assert.Contains(".", email);
    }
    
    public static void ShouldHaveCount<T>(this IEnumerable<T> collection, int expectedCount)
    {
        Assert.Equal(expectedCount, collection.Count());
    }
}
```

## Phase 7: Advanced C# Concepts (Weeks 16-19)

### Week 16: Generics & Advanced Types

**Key Topics:**
- Generic classes and methods
- Constraints on generics
- Covariance and contravariance
- Nullable reference types

**Core Concepts:**
```csharp
public interface IRepository<T> where T : class, IEntity
{
    Task<T> GetByIdAsync(int id);
    Task<IEnumerable<T>> GetAllAsync();
    Task SaveAsync(T entity);
    Task DeleteAsync(int id);
}

public class Repository<T> : IRepository<T> where T : class, IEntity
{
    private readonly DbContext _context;
    
    public Repository(DbContext context)
    {
        _context = context;
    }
    
    public virtual async Task<T> GetByIdAsync(int id)
    {
        return await _context.Set<T>().FindAsync(id);
    }
}

// Testing generics
public class RepositoryTests
{
    [Fact]
    public async Task GetByIdAsync_ExistingEntity_ReturnsEntity()
    {
        // Arrange
        var mockContext = new Mock<DbContext>();
        var mockSet = new Mock<DbSet<User>>();
        
        var user = new User { Id = 1, Name = "Test User" };
        mockSet.Setup(s => s.FindAsync(1)).ReturnsAsync(user);
        mockContext.Setup(c => c.Set<User>()).Returns(mockSet.Object);
        
        var repository = new Repository<User>(mockContext.Object);
        
        // Act
        var result = await repository.GetByIdAsync(1);
        
        // Assert
        Assert.Equal(user, result);
    }
}
```

### Week 17: Delegates, Events & Reflection

**Key Topics:**
- Delegates and multicast delegates
- Events and event handling
- Reflection basics
- Attributes and metadata

**Core Concepts:**
```csharp
public class OrderService
{
    public event EventHandler<OrderProcessedEventArgs> OrderProcessed;
    
    public async Task ProcessOrderAsync(Order order)
    {
        // Process order logic...
        
        // Raise event
        OrderProcessed?.Invoke(this, new OrderProcessedEventArgs(order));
    }
}

public class OrderProcessedEventArgs : EventArgs
{
    public Order Order { get; }
    
    public OrderProcessedEventArgs(Order order)
    {
        Order = order;
    }
}

// Testing events
[Fact]
public async Task ProcessOrderAsync_SuccessfulProcessing_RaisesOrderProcessedEvent()
{
    // Arrange
    var orderService = new OrderService();
    Order capturedOrder = null;
    
    orderService.OrderProcessed += (sender, args) => capturedOrder = args.Order;
    
    var order = new Order { Id = 1, ProductId = 100 };
    
    // Act
    await orderService.ProcessOrderAsync(order);
    
    // Assert
    Assert.NotNull(capturedOrder);
    Assert.Equal(1, capturedOrder.Id);
}
```

### Week 18: Dependency Injection & IoC

**Key Topics:**
- Dependency Injection principles
- IoC containers (built-in .NET DI)
- Service lifetimes (Singleton, Scoped, Transient)
- Testing with DI

**Core Concepts:**
```csharp
// Startup configuration
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<IUserService, UserService>();
    services.AddScoped<IUserRepository, UserRepository>();
    services.AddSingleton<ILogger, ConsoleLogger>();
    services.AddTransient<IEmailService, EmailService>();
}

// Testing with DI
public class UserServiceIntegrationTests
{
    private readonly ServiceProvider _serviceProvider;
    
    public UserServiceIntegrationTests()
    {
        var services = new ServiceCollection();
        services.AddScoped<IUserService, UserService>();
        services.AddScoped<IUserRepository, MockUserRepository>(); // Use mock for testing
        services.AddSingleton<ILogger, TestLogger>();
        
        _serviceProvider = services.BuildServiceProvider();
    }
    
    [Fact]
    public async Task GetUserAsync_WithDI_ReturnsUser()
    {
        // Arrange
        var userService = _serviceProvider.GetRequiredService<IUserService>();
        
        // Act
        var user = await userService.GetUserAsync(1);
        
        // Assert
        Assert.NotNull(user);
    }
}
```

### Week 19: Performance & Memory Management

**Key Topics:**
- Memory management and garbage collection
- IDisposable and using statements
- Span<T> and Memory<T>
- Performance testing

**Core Concepts:**
```csharp
public class FileProcessor : IDisposable
{
    private FileStream _fileStream;
    private bool _disposed = false;
    
    public FileProcessor(string filePath)
    {
        _fileStream = new FileStream(filePath, FileMode.Open);
    }
    
    public void ProcessFile()
    {
        // Process file...
    }
    
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                _fileStream?.Dispose();
            }
            _disposed = true;
        }
    }
}

// Testing disposable objects
[Fact]
public void FileProcessor_Disposed_DoesNotThrow()
{
    // Arrange & Act
    using (var processor = new FileProcessor("test.txt"))
    {
        processor.ProcessFile();
    } // Dispose is called automatically
    
    // Assert - no exception thrown
}
```

## Phase 8: Real-World Applications (Weeks 20-22)

### Week 20: Building a Complete Application

**Project: Task Management System**
- Create models (Task, Project, User)
- Implement business logic services
- Add data persistence layer
- Write comprehensive tests

### Week 21: Web API Development

**Key Topics:**
- ASP.NET Core Web API basics
- Controller testing
- Integration testing
- API documentation

### Week 22: Advanced Testing Patterns

**Key Topics:**
- Test-Driven Development (TDD)
- Behavior-Driven Development (BDD)
- Contract testing
- Performance testing

## Structured Study Plan

### Daily Study Schedule (Recommended: 1-2 hours per day)

**Monday-Wednesday-Friday:** New concept learning
- 30 minutes: Read theory and documentation
- 30 minutes: Code examples and experimentation
- 30 minutes: Practice exercises

**Tuesday-Thursday:** Testing focus
- 45 minutes: Write tests for previous day's code
- 30 minutes: Refactor and improve test coverage
- 15 minutes: Review testing best practices

**Saturday:** Project work
- 2 hours: Work on larger practice projects
- Apply concepts learned during the week
- Focus on integration and real-world scenarios

**Sunday:** Review and preparation
- 1 hour: Review week's progress
- 30 minutes: Plan next week's learning
- 30 minutes: Read additional resources

### Weekly Milestones

**Weeks 1-4:** Complete basic C# console applications with error handling
**Weeks 5-8:** Build object-oriented applications with proper design patterns
**Weeks 9-10:** Achieve 80%+ test coverage on all new code
**Weeks 11-12:** Create data-driven applications with async operations
**Weeks 13-15:** Master mocking and write maintainable test suites
**Weeks 16-19:** Implement advanced C# features in production-quality code
**Weeks 20-22:** Deploy complete applications with comprehensive test coverage

## Recommended Resources

### Books
- "C# in Depth" by Jon Skeet
- "Clean Code" by Robert C. Martin
- "The Art of Unit Testing" by Roy Osherove
- "Effective C#" by Bill Wagner

### Online Resources
- Microsoft Learn C# Path
- Unit Testing in C# Tutorial (Microsoft Docs)
- Moq Documentation and Examples
- Clean Architecture with .NET Core (GitHub repos)

### Tools & Frameworks
- Visual Studio Community / VS Code
- .NET CLI
- xUnit / NUnit / MSTest
- Moq Framework
- FluentAssertions
- Coverlet (code coverage)

## Assessment Criteria

### By End of Program, You Should Be Able To:
1. Write clean, maintainable C# code following SOLID principles
2. Implement comprehensive unit test suites with 90%+ coverage
3. Use mocking frameworks effectively for isolated testing
4. Apply async/await patterns correctly in both code and tests
5. Design testable architectures using dependency injection
6. Debug and troubleshoot both application and test code
7. Refactor legacy code while maintaining test coverage
8. Performance test and optimize C# applications

### Success Metrics
- Complete all practice tasks with accompanying tests
- Build and test 3 substantial projects during the program
- Achieve consistent test coverage above 85%
- Demonstrate TDD workflow in final project
- Code review readiness for production environments

This roadmap will take you approximately 22 weeks to complete, with each phase building upon the previous one. The emphasis on unit testing throughout ensures you develop both coding and testing skills simultaneously, which is crucial for professional software development.