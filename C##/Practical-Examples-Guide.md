# Practical Examples Guide for TxBusinessRest Project

## 🎯 Real-World Examples from the Codebase

This guide provides practical examples from the actual TxBusinessRest project to help you understand how everything works together.

## 📁 Example 1: Complete CRUD Operation Flow

### 1.1 Controller Layer (API Endpoint)
```csharp
// File: TxBusinessRest/Controllers/UsersController.cs
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly IUserService _userService;
    
    public UsersController(IUserService userService)
    {
        _userService = userService;
    }
    
    [HttpGet("{id}")]
    [ProducesResponseType(typeof(UserDTO), 200)]
    [ProducesResponseType(typeof(NotFoundDTO), 404)]
    public async Task<ActionResult<UserDTO>> GetUser(int id)
    {
        try
        {
            var user = await _userService.GetUserAsync(id);
            if (user == null)
                return NotFound(new NotFoundDTO { Message = $"User with ID {id} not found" });
                
            return Ok(user);
        }
        catch (Exception ex)
        {
            // Exception handled by ApiExceptionFilter
            throw;
        }
    }
    
    [HttpPost]
    [ProducesResponseType(typeof(UserDTO), 201)]
    [ProducesResponseType(typeof(BadRequestDTO), 400)]
    public async Task<ActionResult<UserDTO>> CreateUser([FromBody] CreateUserDTO createUserDto)
    {
        var user = await _userService.CreateUserAsync(createUserDto);
        return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
    }
}
```

### 1.2 DTO Layer (Data Transfer Objects)
```csharp
// File: TxBusinessRest/DTOs/UserDTOs/UserDTO.cs
public class UserDTO
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public DateTime CreatedDate { get; set; }
    public bool IsActive { get; set; }
}

// File: TxBusinessRest/DTOs/UserDTOs/CreateUserDTO.cs
public class CreateUserDTO
{
    [Required]
    [StringLength(100, MinimumLength = 2)]
    public string Name { get; set; }
    
    [Required]
    [EmailAddress]
    public string Email { get; set; }
    
    public bool IsActive { get; set; } = true;
}
```

### 1.3 Service Layer (Business Logic)
```csharp
// File: TxBusiness/Services/UserService.cs
public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;
    private readonly IMapper _mapper;
    private readonly ILogger<UserService> _logger;
    
    public UserService(
        IUserRepository userRepository, 
        IMapper mapper, 
        ILogger<UserService> logger)
    {
        _userRepository = userRepository;
        _mapper = mapper;
        _logger = logger;
    }
    
    public async Task<UserDTO> GetUserAsync(int id)
    {
        _logger.LogInformation("Getting user with ID: {UserId}", id);
        
        var user = await _userRepository.GetAsync(id);
        if (user == null)
        {
            _logger.LogWarning("User with ID {UserId} not found", id);
            return null;
        }
        
        return _mapper.Map<UserDTO>(user);
    }
    
    public async Task<UserDTO> CreateUserAsync(CreateUserDTO createUserDto)
    {
        _logger.LogInformation("Creating new user with email: {Email}", createUserDto.Email);
        
        // Business logic validation
        var existingUser = await _userRepository.GetByEmailAsync(createUserDto.Email);
        if (existingUser != null)
        {
            throw new BusinessException("User with this email already exists");
        }
        
        var user = _mapper.Map<User>(createUserDto);
        user.CreatedDate = DateTime.UtcNow;
        
        await _userRepository.InsertAsync(user);
        
        _logger.LogInformation("User created successfully with ID: {UserId}", user.Id);
        return _mapper.Map<UserDTO>(user);
    }
}
```

### 1.4 Repository Layer (Data Access)
```csharp
// File: TxModel/Repositories/UserRepository.cs
public class UserRepository : IUserRepository
{
    private readonly IDbConnection _connection;
    
    public UserRepository(IDbConnection connection)
    {
        _connection = connection;
    }
    
    public async Task<User> GetAsync(int id)
    {
        const string sql = @"
            SELECT Id, Name, Email, CreatedDate, IsActive 
            FROM Users 
            WHERE Id = @Id";
            
        return await _connection.QueryFirstOrDefaultAsync<User>(sql, new { Id = id });
    }
    
    public async Task<User> GetByEmailAsync(string email)
    {
        const string sql = @"
            SELECT Id, Name, Email, CreatedDate, IsActive 
            FROM Users 
            WHERE Email = @Email";
            
        return await _connection.QueryFirstOrDefaultAsync<User>(sql, new { Email = email });
    }
    
    public async Task<int> InsertAsync(User user)
    {
        const string sql = @"
            INSERT INTO Users (Name, Email, CreatedDate, IsActive)
            VALUES (@Name, @Email, @CreatedDate, @IsActive);
            SELECT CAST(SCOPE_IDENTITY() as int);";
            
        return await _connection.QuerySingleAsync<int>(sql, user);
    }
}
```

### 1.5 Model Layer (Domain Entity)
```csharp
// File: TxModel/Models/User.cs
public class User : IIdConcept
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public DateTime CreatedDate { get; set; }
    public bool IsActive { get; set; }
}
```

## 🧪 Example 2: Unit Testing Patterns

### 2.1 Repository Test
```csharp
// File: TxModelTests/Repositories/UserRepositoryTests.cs
[Collection("MsSqlTestCollection")]
public class UserRepositoryTests : IdTests<User, IUserRepository>
{
    public UserRepositoryTests(MsSqlTestFixture fixture) : base()
    {
        // Database setup handled by fixture
    }
    
    [Fact]
    public async Task GetByEmailAsync_ShouldReturnUser_WhenEmailExists()
    {
        // Arrange
        var expectedUser = GetRandomNew();
        expectedUser.Email = "test@example.com";
        Repository.Insert(expectedUser);
        
        // Act
        var actualUser = await Repository.GetByEmailAsync("test@example.com");
        
        // Assert
        Assert.NotNull(actualUser);
        Assert.Equal(expectedUser.Email, actualUser.Email);
        Assert.Equal(expectedUser.Name, actualUser.Name);
    }
    
    [Fact]
    public async Task GetByEmailAsync_ShouldReturnNull_WhenEmailDoesNotExist()
    {
        // Act
        var result = await Repository.GetByEmailAsync("nonexistent@example.com");
        
        // Assert
        Assert.Null(result);
    }
}
```

### 2.2 Service Test with Mocking
```csharp
// File: TxBusinessUnitTests/Services/UserServiceTests.cs
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _mockUserRepository;
    private readonly Mock<IMapper> _mockMapper;
    private readonly Mock<ILogger<UserService>> _mockLogger;
    private readonly UserService _userService;
    
    public UserServiceTests()
    {
        _mockUserRepository = new Mock<IUserRepository>();
        _mockMapper = new Mock<IMapper>();
        _mockLogger = new Mock<ILogger<UserService>>();
        
        _userService = new UserService(
            _mockUserRepository.Object, 
            _mockMapper.Object, 
            _mockLogger.Object);
    }
    
    [Fact]
    public async Task GetUserAsync_ShouldReturnUser_WhenIdExists()
    {
        // Arrange
        var userId = 1;
        var user = new User { Id = userId, Name = "John Doe", Email = "john@example.com" };
        var userDto = new UserDTO { Id = userId, Name = "John Doe", Email = "john@example.com" };
        
        _mockUserRepository.Setup(x => x.GetAsync(userId)).ReturnsAsync(user);
        _mockMapper.Setup(x => x.Map<UserDTO>(user)).Returns(userDto);
        
        // Act
        var result = await _userService.GetUserAsync(userId);
        
        // Assert
        Assert.NotNull(result);
        Assert.Equal(userId, result.Id);
        Assert.Equal("John Doe", result.Name);
        
        _mockUserRepository.Verify(x => x.GetAsync(userId), Times.Once);
        _mockMapper.Verify(x => x.Map<UserDTO>(user), Times.Once);
    }
    
    [Fact]
    public async Task CreateUserAsync_ShouldThrowException_WhenEmailAlreadyExists()
    {
        // Arrange
        var createUserDto = new CreateUserDTO 
        { 
            Name = "John Doe", 
            Email = "john@example.com" 
        };
        
        var existingUser = new User { Id = 1, Email = "john@example.com" };
        _mockUserRepository.Setup(x => x.GetByEmailAsync("john@example.com"))
                          .ReturnsAsync(existingUser);
        
        // Act & Assert
        await Assert.ThrowsAsync<BusinessException>(
            () => _userService.CreateUserAsync(createUserDto));
    }
}
```

### 2.3 Controller Test
```csharp
// File: TxBusinessUnitTests/Controllers/UsersControllerTests.cs
public class UsersControllerTests
{
    private readonly Mock<IUserService> _mockUserService;
    private readonly UsersController _controller;
    
    public UsersControllerTests()
    {
        _mockUserService = new Mock<IUserService>();
        _controller = new UsersController(_mockUserService.Object);
    }
    
    [Fact]
    public async Task GetUser_ShouldReturnOk_WhenUserExists()
    {
        // Arrange
        var userId = 1;
        var userDto = new UserDTO { Id = userId, Name = "John Doe" };
        _mockUserService.Setup(x => x.GetUserAsync(userId)).ReturnsAsync(userDto);
        
        // Act
        var result = await _controller.GetUser(userId);
        
        // Assert
        var okResult = Assert.IsType<OkObjectResult>(result.Result);
        var returnedUser = Assert.IsType<UserDTO>(okResult.Value);
        Assert.Equal(userId, returnedUser.Id);
    }
    
    [Fact]
    public async Task GetUser_ShouldReturnNotFound_WhenUserDoesNotExist()
    {
        // Arrange
        var userId = 999;
        _mockUserService.Setup(x => x.GetUserAsync(userId)).ReturnsAsync((UserDTO)null);
        
        // Act
        var result = await _controller.GetUser(userId);
        
        // Assert
        var notFoundResult = Assert.IsType<NotFoundObjectResult>(result.Result);
        var errorDto = Assert.IsType<NotFoundDTO>(notFoundResult.Value);
        Assert.Contains("not found", errorDto.Message);
    }
}
```

## 🔧 Example 3: Configuration and Dependency Injection

### 3.1 Startup Configuration
```csharp
// File: TxBusinessRest/Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    // Add controllers with JSON options
    services.AddControllers(options =>
    {
        options.Filters.Add(typeof(ApiExceptionFilter));
        options.Filters.Add(new AuthorizeFilter());
    })
    .AddJsonOptions(opts =>
    {
        opts.JsonSerializerOptions.Converters.Add(new JsonStringEnumConverter());
        opts.JsonSerializerOptions.Converters.Add(new DateTimeConverter());
        opts.JsonSerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
    });
    
    // Add AutoMapper
    services.AddAutoMapper(typeof(MappingProfile));
    
    // Add business services
    services.AddTxObject(txConfiguration, StopApplication, configurationFile);
    
    // Add Swagger
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "TxBusinessRest API", Version = "v1" });
    });
}
```

### 3.2 Service Registration Extension
```csharp
// File: TxBusiness/Extensions/IServiceCollectionExtension.cs
public static class IServiceCollectionExtension
{
    public static IServiceCollection AddTxObject(
        this IServiceCollection services, 
        ConfigurationReader config, 
        Action stopApplication, 
        string configFile)
    {
        // Register repositories
        services.AddScoped<IUserRepository, UserRepository>();
        services.AddScoped<IObjectRepository, ObjectRepository>();
        
        // Register services
        services.AddScoped<IUserService, UserService>();
        services.AddScoped<IObjectService, ObjectService>();
        
        // Register piloters
        services.AddScoped<IUserPiloter, UserPiloter>();
        
        return services;
    }
}
```

## 📊 Example 4: Data Access Patterns

### 4.1 Complex Query with Dapper
```csharp
// File: TxModel/Repositories/ObjectRepository.cs
public class ObjectRepository : IObjectRepository
{
    private readonly IDbConnection _connection;
    
    public async Task<IEnumerable<ObjectWithAttributes>> GetObjectsWithAttributesAsync(int objectTypeId)
    {
        const string sql = @"
            SELECT 
                o.Id,
                o.Name,
                o.CreatedDate,
                a.Id as AttributeId,
                a.Name as AttributeName,
                a.DataType,
                d.Value as AttributeValue
            FROM Objects o
            INNER JOIN ObjectAttributes oa ON o.Id = oa.ObjectId
            INNER JOIN Attributes a ON oa.AttributeId = a.Id
            LEFT JOIN Data d ON o.Id = d.ObjectId AND a.Id = d.AttributeId
            WHERE o.ObjectTypeId = @ObjectTypeId
            ORDER BY o.Name, a.Name";
            
        var objectDict = new Dictionary<int, ObjectWithAttributes>();
        
        await _connection.QueryAsync<ObjectWithAttributes, AttributeData, ObjectWithAttributes>(
            sql,
            (obj, attr) =>
            {
                if (!objectDict.TryGetValue(obj.Id, out var existingObj))
                {
                    existingObj = obj;
                    existingObj.Attributes = new List<AttributeData>();
                    objectDict.Add(obj.Id, existingObj);
                }
                
                if (attr != null)
                    existingObj.Attributes.Add(attr);
                    
                return existingObj;
            },
            new { ObjectTypeId = objectTypeId },
            splitOn: "AttributeId");
            
        return objectDict.Values;
    }
}
```

### 4.2 Unit of Work Pattern
```csharp
// File: TxModel/UnitOfWork.cs
public class UnitOfWork : IUnitOfWork
{
    private readonly IDbConnection _connection;
    private readonly IDbTransaction _transaction;
    private readonly Dictionary<Type, object> _repositories;
    
    public UnitOfWork(IDbConnection connection)
    {
        _connection = connection;
        _transaction = _connection.BeginTransaction();
        _repositories = new Dictionary<Type, object>();
    }
    
    public T CreateRepository<T>() where T : class
    {
        var type = typeof(T);
        
        if (!_repositories.ContainsKey(type))
        {
            var repository = Activator.CreateInstance(type, _connection) as T;
            _repositories.Add(type, repository);
        }
        
        return (T)_repositories[type];
    }
    
    public void Commit()
    {
        _transaction.Commit();
    }
    
    public void Rollback()
    {
        _transaction.Rollback();
    }
    
    public void Dispose()
    {
        _transaction?.Dispose();
        _connection?.Dispose();
    }
}
```

## 🚀 Example 5: Error Handling and Validation

### 5.1 Custom Exception Filter
```csharp
// File: TxBusinessRest/Filters/ApiExceptionFilter.cs
public class ApiExceptionFilter : IExceptionFilter
{
    private readonly ILogger<ApiExceptionFilter> _logger;
    
    public ApiExceptionFilter(ILogger<ApiExceptionFilter> logger)
    {
        _logger = logger;
    }
    
    public void OnException(ExceptionContext context)
    {
        _logger.LogError(context.Exception, "An unhandled exception occurred");
        
        var response = context.Exception switch
        {
            BusinessException ex => new BadRequestObjectResult(new BadRequestDTO 
            { 
                Message = ex.Message,
                ErrorCode = ex.ErrorCode 
            }),
            NotFoundException ex => new NotFoundObjectResult(new NotFoundDTO 
            { 
                Message = ex.Message 
            }),
            ValidationException ex => new BadRequestObjectResult(new ValidationErrorDTO 
            { 
                Message = "Validation failed",
                Errors = ex.Errors 
            }),
            _ => new ObjectResult(new ErrorDTO 
            { 
                Message = "An unexpected error occurred" 
            })
            {
                StatusCode = 500
            }
        };
        
        context.Result = response;
        context.ExceptionHandled = true;
    }
}
```

### 5.2 Validation with FluentValidation
```csharp
// File: TxBusinessRest/Validators/CreateUserValidator.cs
public class CreateUserValidator : AbstractValidator<CreateUserDTO>
{
    public CreateUserValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Name is required")
            .Length(2, 100).WithMessage("Name must be between 2 and 100 characters");
            
        RuleFor(x => x.Email)
            .NotEmpty().WithMessage("Email is required")
            .EmailAddress().WithMessage("Email must be a valid email address");
            
        RuleFor(x => x.Email)
            .MustAsync(BeUniqueEmail).WithMessage("Email already exists");
    }
    
    private async Task<bool> BeUniqueEmail(string email, CancellationToken token)
    {
        // Check if email is unique
        // This would typically call a service or repository
        return await Task.FromResult(true); // Simplified for example
    }
}
```

## 🔐 Example 6: Authentication and Authorization

### 6.1 JWT Authentication Setup
```csharp
// File: TxBusinessRest/Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    // JWT Authentication
    services.AddTxJwtBearerAuthentication(
        JwtUtilsDefaults.validAudience, 
        JwtUtilsDefaults.validAudience.Any(), 
        _certificatesHolder);
        
    services.AddAuthorization(options =>
    {
        options.AddPolicy("AdminOnly", policy => 
            policy.RequireRole("Admin"));
        options.AddPolicy("UserOrAdmin", policy => 
            policy.RequireRole("User", "Admin"));
    });
}
```

### 6.2 Authorized Controller Methods
```csharp
// File: TxBusinessRest/Controllers/UsersController.cs
[Authorize] // All methods require authentication
public class UsersController : ControllerBase
{
    [HttpGet]
    [Authorize(Policy = "UserOrAdmin")]
    public async Task<ActionResult<IEnumerable<UserDTO>>> GetUsers()
    {
        // Get all users - requires User or Admin role
    }
    
    [HttpDelete("{id}")]
    [Authorize(Policy = "AdminOnly")]
    public async Task<ActionResult> DeleteUser(int id)
    {
        // Delete user - requires Admin role only
    }
}
```

## 📈 Example 7: Logging and Monitoring

### 7.1 Structured Logging
```csharp
// File: TxBusiness/Services/UserService.cs
public class UserService : IUserService
{
    private readonly ILogger<UserService> _logger;
    
    public async Task<UserDTO> GetUserAsync(int id)
    {
        using var scope = _logger.BeginScope(new Dictionary<string, object>
        {
            ["UserId"] = id,
            ["Operation"] = "GetUser"
        });
        
        _logger.LogInformation("Getting user with ID: {UserId}", id);
        
        try
        {
            var user = await _userRepository.GetAsync(id);
            
            if (user == null)
            {
                _logger.LogWarning("User with ID {UserId} not found", id);
                return null;
            }
            
            _logger.LogInformation("User retrieved successfully: {UserName}", user.Name);
            return _mapper.Map<UserDTO>(user);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error retrieving user with ID {UserId}", id);
            throw;
        }
    }
}
```

## 🎯 Example 8: API Documentation with Swagger

### 8.1 Swagger Configuration
```csharp
// File: TxBusinessRest/Startup.cs
private void SwaggerGen(SwaggerGenOptions options)
{
    options.SwaggerDoc("v1", new OpenApiInfo 
    { 
        Title = "TxBusinessRest API", 
        Version = "v1",
        Description = "REST API for TEEXMA business operations",
        Contact = new OpenApiContact
        {
            Name = "Team TxDev",
            Email = "-informatique@bassetti-group.com"
        }
    });
    
    // Add JWT authentication to Swagger
    options.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
    {
        Name = "Authorization",
        Type = SecuritySchemeType.ApiKey,
        Scheme = "Bearer",
        BearerFormat = "JWT",
        In = ParameterLocation.Header,
        Description = "Enter 'Bearer' [space] and then your valid token"
    });
}
```

### 8.2 Controller Documentation
```csharp
// File: TxBusinessRest/Controllers/UsersController.cs
[ApiController]
[Route("api/[controller]")]
[Produces("application/json")]
public class UsersController : ControllerBase
{
    /// <summary>
    /// Gets a user by their unique identifier
    /// </summary>
    /// <param name="id">The user identifier</param>
    /// <returns>A user object</returns>
    /// <response code="200">Returns the requested user</response>
    /// <response code="404">User not found</response>
    /// <response code="401">Unauthorized</response>
    [HttpGet("{id}")]
    [ProducesResponseType(typeof(UserDTO), 200)]
    [ProducesResponseType(typeof(NotFoundDTO), 404)]
    [ProducesResponseType(401)]
    public async Task<ActionResult<UserDTO>> GetUser(int id)
    {
        // Implementation
    }
}
```

## 🚀 Getting Started Checklist

1. **Set up development environment** (Visual Studio, SQL Server)
2. **Clone and build the project**
3. **Configure database connection**
4. **Run the application** and access Swagger UI
5. **Study the examples** in this guide
6. **Write your first test** following the patterns
7. **Add a new endpoint** using the established patterns
8. **Debug and troubleshoot** using the logging examples

---

*These practical examples are based on the actual TxBusinessRest codebase. Use them as templates for your own development and testing.*
