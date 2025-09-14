# TxBusinessRest Project Architecture & Setup Guide

## 🏗️ Project Architecture Overview

The TxBusinessRest project follows a **layered architecture** pattern with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                      │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐ │
│  │   Controllers   │  │      DTOs       │  │  Filters    │ │
│  │   (33 files)    │  │   (230+ files)  │  │  (4 files)  │ │
│  └─────────────────┘  └─────────────────┘  └─────────────┘ │
└─────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────┐
│                     BUSINESS LAYER                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐ │
│  │    Services     │  │    Piloters     │  │   Helpers   │ │
│  │   (73 files)    │  │   (44 files)    │  │  (9 files)  │ │
│  └─────────────────┘  └─────────────────┘  └─────────────┘ │
└─────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────┐
│                      DATA LAYER                            │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐ │
│  │  Repositories   │  │     Models      │  │  UnitOfWork │ │
│  │  (143 files)    │  │   (99 files)    │  │  (1 file)   │ │
│  └─────────────────┘  └─────────────────┘  └─────────────┘ │
└─────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────┐
│                    DATABASE LAYER                          │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐ │
│  │   SQL Server    │  │   PostgreSQL    │  │   Dapper    │ │
│  │   (Primary)     │  │   (Supported)   │  │  (ORM)      │ │
│  └─────────────────┘  └─────────────────┘  └─────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 Project Setup Guide

### Prerequisites

1. **Visual Studio 2022** (Community/Professional/Enterprise)
   - .NET 8.0 SDK
   - SQL Server Management Studio (SSMS)
   - Git for Windows

2. **Database Setup**
   - SQL Server 2017+ or PostgreSQL 12+
   - Database: `ACOU_ReadyToUse` (as per appsettings.json)

3. **Development Tools**
   - Docker Desktop (for test database)
   - Postman (for API testing)
   - Swagger UI (built-in)

### Step 1: Clone and Setup

```bash
# Clone the repository
git clone https://github.com/BASSETTI-GROUP/tx-business-rest.git
cd tx-business-rest

# Restore NuGet packages
dotnet restore

# Build the solution
dotnet build
```

### Step 2: Database Configuration

#### Option A: SQL Server (Recommended)
```json
// appsettings.Development.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=YOUR_SERVER;Initial Catalog=ACOU_ReadyToUse;Integrated Security=True"
  }
}
```

#### Option B: Docker Test Database
```powershell
# Navigate to test database scripts
cd TxModelTests\Databases

# Start SQL Server container
.\StartServer.ps1

# Connection details:
# Server: localhost,1415
# Username: SA
# Password: TxModel2022
# Database: TxModelTests
```

### Step 3: Configuration Files

#### TEEXMA Configuration
The project requires TEEXMA configuration files:

```
TxBusinessRest/
├── TEEXMA_FILES/
│   ├── Configuration/
│   │   ├── TEEXMA.exml          # Encrypted config (Production)
│   │   ├── TEEXMA.xml           # Plain config (Development)
│   │   └── VerificationCertificate.cer
│   └── Core/
│       └── [XSD files]
```

#### Environment Setup
```json
// appsettings.Development.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=L913\\SQLE2017;Initial Catalog=ACOU_ReadyToUse;Integrated Security=True"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```

### Step 4: Running the Application

```bash
# Run the main API
dotnet run --project TxBusinessRest

# Or run from Visual Studio
# Set TxBusinessRest as startup project and press F5
```

**Access Points:**
- **API**: `https://localhost:44336` or `http://localhost:64264`
- **Swagger UI**: `https://localhost:44336/index.html`
- **Documentation**: `https://localhost:44336/docs`

## 🏛️ Architecture Deep Dive

### 1. Presentation Layer (TxBusinessRest)

#### Controllers
```csharp
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
    public async Task<ActionResult<UserDTO>> GetUser(int id)
    {
        var user = await _userService.GetUserAsync(id);
        return Ok(user);
    }
}
```

#### DTOs (Data Transfer Objects)
```csharp
public class UserDTO
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public DateTime CreatedDate { get; set; }
}
```

#### Filters
```csharp
public class ApiExceptionFilter : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        // Global exception handling
    }
}
```

### 2. Business Layer (TxBusiness)

#### Services
```csharp
public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;
    private readonly IMapper _mapper;
    
    public UserService(IUserRepository userRepository, IMapper mapper)
    {
        _userRepository = userRepository;
        _mapper = mapper;
    }
    
    public async Task<UserDTO> GetUserAsync(int id)
    {
        var user = await _userRepository.GetAsync(id);
        return _mapper.Map<UserDTO>(user);
    }
}
```

#### Piloters (Business Logic Controllers)
```csharp
public class UserPiloter
{
    private readonly IUserService _userService;
    
    public UserPiloter(IUserService userService)
    {
        _userService = userService;
    }
    
    public async Task<Result<UserDTO>> CreateUserAsync(CreateUserRequest request)
    {
        // Business logic validation
        // Service orchestration
        // Error handling
    }
}
```

### 3. Data Layer (TxModel)

#### Repositories
```csharp
public class UserRepository : IUserRepository
{
    private readonly IDbConnection _connection;
    
    public UserRepository(IDbConnection connection)
    {
        _connection = connection;
    }
    
    public async Task<User> GetAsync(int id)
    {
        const string sql = "SELECT * FROM Users WHERE Id = @Id";
        return await _connection.QueryFirstOrDefaultAsync<User>(sql, new { Id = id });
    }
}
```

#### Models
```csharp
public class User : IIdConcept
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public DateTime CreatedDate { get; set; }
}
```

#### Unit of Work
```csharp
public class UnitOfWork : IUnitOfWork
{
    private readonly IDbConnection _connection;
    private readonly Dictionary<Type, object> _repositories;
    
    public T CreateRepository<T>() where T : class
    {
        // Repository factory pattern
    }
    
    public void Dispose()
    {
        _connection?.Dispose();
    }
}
```

## 🔧 Dependency Injection Setup

### Startup.cs Configuration
```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Controllers
    services.AddControllers();
    
    // AutoMapper
    services.AddAutoMapper(typeof(MappingProfile));
    
    // Business Services
    services.AddTxObject(txConfiguration, StopApplication, configurationFile);
    services.AddTxAdvancedSearch(txConfiguration, StopApplication, configurationFile);
    services.AddTxHourTracking(txConfiguration, StopApplication, configurationFile);
    
    // Swagger
    services.AddSwaggerGen();
}
```

### Service Registration Patterns
```csharp
// In extension methods
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddTxObject(
        this IServiceCollection services, 
        ConfigurationReader config, 
        Action stopApplication, 
        string configFile)
    {
        services.AddScoped<IUserService, UserService>();
        services.AddScoped<IUserRepository, UserRepository>();
        return services;
    }
}
```

## 🧪 Testing Architecture

### Test Project Structure
```
Tests/
├── TxModelTests/           # Data layer tests
│   ├── Repositories/       # Repository unit tests
│   ├── Fixtures/          # Database fixtures
│   └── Utilities/         # Test utilities
├── TxBusinessUnitTests/    # Business layer tests
│   ├── Piloters/          # Service tests
│   └── Services/          # Business logic tests
└── IntegrationTests/       # End-to-end tests
```

### Test Configuration
```csharp
// Test project file
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <IsPackable>false</IsPackable>
    <IsTestProject>true</IsTestProject>
  </PropertyGroup>
  
  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.13.0" />
    <PackageReference Include="xunit" Version="2.9.3" />
    <PackageReference Include="Moq" Version="4.20.72" />
    <PackageReference Include="AutoFixture" Version="4.18.1" />
  </ItemGroup>
</Project>
```

## 🔐 Security Architecture

### JWT Authentication
```csharp
// JWT Configuration
services.AddTxJwtBearerAuthentication(
    JwtUtilsDefaults.validAudience, 
    JwtUtilsDefaults.validAudience.Any(), 
    _certificatesHolder);
```

### Authorization
```csharp
[Authorize] // All controllers require authentication
public class UsersController : ControllerBase
{
    // Controller methods
}
```

### CORS Configuration
```csharp
app.UseMiddleware<CustomCorsMiddleware>(Options.Create(_validUrls));
```

## 📊 Data Flow Architecture

### Request Flow
```
1. HTTP Request → Controller
2. Controller → Service (Business Logic)
3. Service → Repository (Data Access)
4. Repository → Database
5. Database → Repository → Service → Controller → HTTP Response
```

### Error Handling Flow
```
1. Exception occurs in any layer
2. Caught by ApiExceptionFilter
3. Converted to appropriate HTTP status
4. Returned as JSON error response
```

## 🚀 Development Workflow

### 1. **Feature Development**
```bash
# Create feature branch
git checkout -b feature/new-user-endpoint

# Make changes
# Add tests
# Run tests
dotnet test

# Commit changes
git add .
git commit -m "Add new user endpoint"
```

### 2. **Testing Workflow**
```bash
# Run all tests
dotnet test

# Run specific test project
dotnet test TxModelTests

# Run with coverage
dotnet test --collect:"XPlat Code Coverage"
```

### 3. **Build and Deploy**
```bash
# Build solution
dotnet build --configuration Release

# Publish for deployment
dotnet publish TxBusinessRest --configuration Release --output ./publish
```

## 🔧 Configuration Management

### Environment-Specific Settings
- **Development**: `appsettings.Development.json`
- **Production**: `appsettings.json`
- **Testing**: Test-specific configuration in test projects

### TEEXMA Configuration
- **Encrypted**: `TEEXMA.exml` (Production)
- **Plain**: `TEEXMA.xml` (Development)
- **Certificates**: For JWT validation

## 📚 Key Design Patterns

### 1. **Repository Pattern**
- Abstracts data access logic
- Enables easy testing with mocks
- Provides consistent data access interface

### 2. **Unit of Work Pattern**
- Manages database transactions
- Coordinates multiple repositories
- Ensures data consistency

### 3. **Dependency Injection**
- Loose coupling between components
- Easy testing and maintenance
- Configuration-driven service registration

### 4. **DTO Pattern**
- Separates internal models from API contracts
- Enables versioning and evolution
- Provides data validation and transformation

## 🚨 Common Issues and Solutions

### 1. **Database Connection Issues**
```csharp
// Problem: Connection string not found
// Solution: Check appsettings.json configuration

// Problem: Database not accessible
// Solution: Verify SQL Server is running and accessible
```

### 2. **TEEXMA Configuration Issues**
```csharp
// Problem: Configuration file not found
// Solution: Ensure TEEXMA.exml or TEEXMA.xml exists

// Problem: Certificate validation fails
// Solution: Check VerificationCertificate.cer file
```

### 3. **Dependency Injection Issues**
```csharp
// Problem: Service not registered
// Solution: Add service registration in Startup.cs

// Problem: Circular dependency
// Solution: Refactor to remove circular references
```

## 🎯 Next Steps for Development

1. **Understand the Domain**: Learn about TEEXMA's business model
2. **Explore Controllers**: Start with simple CRUD operations
3. **Study Services**: Understand business logic patterns
4. **Examine Repositories**: Learn data access patterns
5. **Write Tests**: Practice with existing test patterns
6. **Add Features**: Implement new functionality following established patterns

---

*This architecture guide provides a comprehensive overview of the TxBusinessRest project structure. Use it as a reference while developing and maintaining the application.*
