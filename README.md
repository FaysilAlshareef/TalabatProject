# Talabat E-Commerce API Project

A comprehensive e-commerce RESTful API built with ASP.NET Core, implementing clean architecture principles and industry-standard design patterns. This project provides a robust backend for an online marketplace with features including product management, user authentication, shopping cart, order processing, and payment integration.

## 🌟 Features

- **Product Management**: Browse products with filtering, sorting, and pagination
- **User Authentication & Authorization**: JWT-based authentication with ASP.NET Core Identity
- **Shopping Cart**: Redis-based shopping cart with real-time updates
- **Order Management**: Complete order processing workflow
- **Payment Integration**: Stripe payment gateway integration
- **Image Handling**: Static file serving for product images
- **Caching**: Redis-based response caching for improved performance
- **API Documentation**: Swagger/OpenAPI integration
- **CORS Support**: Configured for Angular frontend integration
- **Error Handling**: Global exception middleware for consistent error responses

## 🏗️ Architecture & Design Patterns

### Onion Architecture (Clean Architecture)
The project follows Onion Architecture principles with clear separation of concerns:

```
Talabat.Core (Domain Layer)
    ↑
Talabat.Repository (Infrastructure Layer)
    ↑
Talabat.Service (Application Layer)
    ↑
Talabat.Apis (Presentation Layer)
```

### Design Patterns Implemented

1. **Repository Pattern**: Abstracts data access logic
2. **Unit of Work Pattern**: Manages transactions across multiple repositories
3. **Specification Pattern**: Encapsulates query logic for reusable and testable queries
4. **Dependency Injection**: Used throughout for loose coupling
5. **AutoMapper**: For object-to-object mapping
6. **Generic Repository**: Reduces code duplication for CRUD operations

## 📋 System Requirements

### Prerequisites

- **.NET SDK 6.0 or higher** - [Download](https://dotnet.microsoft.com/download/dotnet/6.0)
- **SQL Server** (2019 or higher) or **SQL Server Express** - [Download](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
- **Redis Server** - [Download](https://redis.io/download)
- **Visual Studio 2022** (optional but recommended) - [Download](https://visualstudio.microsoft.com/)
  - OR **Visual Studio Code** with C# extension
- **Git** - [Download](https://git-scm.com/downloads)

### Optional
- **Docker Desktop** (for containerized deployment) - [Download](https://www.docker.com/products/docker-desktop)
- **Postman** or similar API testing tool - [Download](https://www.postman.com/)

## 🔧 Dependencies

### Core Technologies
- **ASP.NET Core 6.0** - Web framework
- **Entity Framework Core 6.0.15** - ORM for database operations
- **Microsoft.AspNetCore.Identity.EntityFrameworkCore 6.0.15** - Identity management

### Third-Party Libraries
- **StackExchange.Redis 2.6.96** - Redis client for caching
- **AutoMapper.Extensions.Microsoft.DependencyInjection 12.0.0** - Object mapping
- **Stripe.net 41.6.0** - Payment processing
- **Swashbuckle.AspNetCore 5.6.3** - API documentation (Swagger)
- **Microsoft.AspNetCore.Authentication.JwtBearer 6.0.15** - JWT authentication

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/FaysilAlshareef/TalabatProject.git
cd TalabatProject
```

### 2. Install Dependencies

```bash
dotnet restore
```

### 3. Configure SQL Server

Make sure SQL Server is running on your machine. Update the connection strings in `Talabat.Apis/appsettings.json`:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=.;Database=TalabatProjectDB;Trusted_Connection=True;MultipleActiveResultSets=True",
  "IdentityConnection": "Server=.;Database=TalabatIdentityDB;Trusted_Connection=True;MultipleActiveResultSets=True",
  "Redis": "localhost"
}
```

**For non-Windows or remote SQL Server:**
```json
"DefaultConnection": "Server=your-server;Database=TalabatProjectDB;User Id=your-username;Password=your-password;MultipleActiveResultSets=True"
```

### 4. Install and Start Redis

**Windows:**
- Download Redis for Windows from [Microsoft Archive](https://github.com/microsoftarchive/redis/releases)
- Install and run: `redis-server.exe`

**Linux/macOS:**
```bash
# Install Redis
sudo apt-get install redis-server  # Ubuntu/Debian
brew install redis                  # macOS

# Start Redis
redis-server
```

**Verify Redis is running:**
```bash
redis-cli ping
# Should return: PONG
```

### 5. Configure JWT Settings

Update JWT settings in `appsettings.json` (or use default for development):

```json
"JWT": {
  "Key": "Your-Secret-Key-Here-Must-Be-At-Least-32-Characters",
  "ValidIssuer": "https://localhost:5001/",
  "ValidAudience": "MySecuredApiUsers",
  "DurationInDays": "30"
}
```

### 6. Configure Stripe (Optional - for payment features)

If you want to test payment functionality, add your Stripe keys:

```json
"StripeSettings": {
  "PublishableKey": "your-stripe-publishable-key",
  "SecretKey": "your-stripe-secret-key"
}
```

Get test keys from [Stripe Dashboard](https://dashboard.stripe.com/test/apikeys)

### 7. Apply Migrations and Seed Data

The application automatically applies migrations and seeds data on startup. However, you can do it manually:

```bash
# Navigate to the API project
cd Talabat.Apis

# Apply migrations
dotnet ef database update --context StoreContext
dotnet ef database update --context AppIdentityDbContext

# Or simply run the application (migrations run automatically)
dotnet run
```

### 8. Run the Application

```bash
cd Talabat.Apis
dotnet run
```

The API will be available at:
- HTTPS: `https://localhost:5001`
- HTTP: `http://localhost:5000`

### 9. Access Swagger Documentation

Once the application is running, navigate to:
```
https://localhost:5001/swagger
```

This provides interactive API documentation where you can test all endpoints.

## 🐳 Running with Docker

```bash
# Build the Docker image
docker build -t talabat-api -f Talabat.Apis/Dockerfile .

# Run the container
docker run -p 5000:80 talabat-api
```

**Note:** Make sure to configure connection strings to point to accessible SQL Server and Redis instances when running in Docker.

## 📁 Project Structure

```
TalabatProject/
├── Talabat.Core/                    # Domain Layer
│   ├── Entities/                    # Domain entities (Product, Order, etc.)
│   ├── Repositories/                # Repository interfaces
│   ├── Services/                    # Service interfaces
│   └── Specifications/              # Specification pattern implementations
│
├── Talabat.Repository/              # Infrastructure Layer
│   ├── Data/
│   │   ├── Contexts/                # DbContext classes
│   │   ├── Config/                  # Entity configurations
│   │   └── Migrations/              # EF Core migrations
│   ├── Identity/                    # Identity DbContext and configurations
│   └── Repositories/                # Repository implementations
│
├── Talabat.Service/                 # Application Layer
│   └── Services/                    # Service implementations
│
└── Talabat.Apis/                    # Presentation Layer
    ├── Controllers/                 # API Controllers
    ├── Dtos/                        # Data Transfer Objects
    ├── Errors/                      # Error handling
    ├── Extensions/                  # Service extensions
    ├── Helpers/                     # Helper classes (AutoMapper profiles)
    ├── Middlewares/                 # Custom middlewares
    └── wwwroot/                     # Static files (product images)
```

## 🔑 Key Components Explained

### Controllers
- **ProductController**: Product catalog operations (GET, filtering, pagination)
- **AccountController**: User registration, login, and profile management
- **BasketController**: Shopping cart operations
- **OrdersController**: Order creation and management
- **PaymentsController**: Payment processing with Stripe

### Database Contexts
- **StoreContext**: Main application data (products, orders, etc.)
- **AppIdentityDbContext**: User authentication and identity data

### Services
- **ITokenServices**: JWT token generation
- **IOrderService**: Order processing logic
- **IPaymentService**: Payment processing with Stripe
- **IResponseCacheService**: Redis caching operations

## 🧪 Testing the API

### Example Endpoints

**Get Products:**
```http
GET https://localhost:5001/api/products
GET https://localhost:5001/api/products?pageIndex=1&pageSize=5
GET https://localhost:5001/api/products?brandId=1&typeId=1
```

**Register User:**
```http
POST https://localhost:5001/api/account/register
Content-Type: application/json

{
  "displayName": "John Doe",
  "email": "john@example.com",
  "password": "Pa$$w0rd"
}
```

**Login:**
```http
POST https://localhost:5001/api/account/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "Pa$$w0rd"
}
```

## 🛠️ Development

### Building the Project

```bash
dotnet build
```

### Running Tests

```bash
dotnet test
```

### Creating New Migrations

```bash
# For Store database
dotnet ef migrations add MigrationName --context StoreContext --project Talabat.Repository --startup-project Talabat.Apis

# For Identity database
dotnet ef migrations add MigrationName --context AppIdentityDbContext --project Talabat.Repository --startup-project Talabat.Apis
```

## 🔒 Security Considerations

- **JWT Tokens**: Secure your JWT secret key in production
- **Stripe Keys**: Never commit real Stripe keys to source control
- **Connection Strings**: Use environment variables or Azure Key Vault in production
- **CORS**: Update CORS policy for production frontend URL

## 🌐 CORS Configuration

The API is configured to accept requests from `http://localhost:4200` (Angular default). Update in `Program.cs` for your frontend:

```csharp
options.AddPolicy("CorsPolicy", options =>
{
    options.AllowAnyHeader()
           .AllowAnyMethod()
           .WithOrigins("http://localhost:4200"); // Update this
});
```

## 📝 Additional Resources

- [ASP.NET Core Documentation](https://docs.microsoft.com/en-us/aspnet/core/)
- [Entity Framework Core Documentation](https://docs.microsoft.com/en-us/ef/core/)
- [Redis Documentation](https://redis.io/documentation)
- [Stripe API Documentation](https://stripe.com/docs/api)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is developed for educational purposes.

## 👤 Author

Faysil Alshareef

## 🙏 Acknowledgments

Built with ASP.NET Core following clean architecture principles and best practices for building scalable, maintainable e-commerce applications.
