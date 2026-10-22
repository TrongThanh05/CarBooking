# CarBooking

CarBooking is an ASP.NET Core MVC application for building a car-booking website. The project targets .NET 8 and includes:

- ASP.NET Core MVC with Razor views
- ASP.NET Core Identity for account registration and sign-in
- Entity Framework Core with SQL Server
- A pre-created Identity database migration
- Bootstrap, jQuery, and client-side validation assets

## Prerequisites

Install the following software before generating or running the project:

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- SQL Server LocalDB (included with Visual Studio) or another SQL Server instance
- Visual Studio 2022 with the ASP.NET and web development workload, or an editor such as Visual Studio Code

Verify the .NET SDK installation:

```bash
dotnet --version
```

## Generate a new project

The current project can be generated from the ASP.NET Core MVC template with Individual Identity authentication:

```bash
dotnet new mvc \
  --name CarBooking \
  --framework net8.0 \
  --auth Individual
cd CarBooking
```

On Windows PowerShell, the same command can be written on one line:

```powershell
dotnet new mvc --name CarBooking --framework net8.0 --auth Individual
Set-Location .\CarBooking
```

Restore the project dependencies:

```bash
dotnet restore
```

If the template does not add the required Entity Framework Core packages, install the packages used by this project:

```bash
dotnet add package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore --version 8.0.22
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore --version 8.0.22
dotnet add package Microsoft.AspNetCore.Identity.UI --version 8.0.22
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 8.0.22
dotnet add package Microsoft.EntityFrameworkCore.Tools --version 8.0.22
```

## Configure the database

The development configuration uses SQL Server LocalDB. The default connection string is stored in `appsettings.json`:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=aspnet-CarBooking-7c199888-afa3-45d4-8d9c-362a0177e964;Trusted_Connection=True;MultipleActiveResultSets=true"
}
```

For another SQL Server instance, replace the value of `DefaultConnection`. Do not commit production passwords or other secrets to `appsettings.json`; use [user secrets](https://learn.microsoft.com/aspnet/core/security/app-secrets) or environment variables instead.

Create or update the Identity database with Entity Framework Core:

```bash
dotnet tool install --global dotnet-ef
dotnet ef database update
```

If the database schema changes, create a migration and apply it:

```bash
dotnet ef migrations add DescribeYourChange
dotnet ef database update
```

## Run the application

Start the application from the project directory:

```bash
dotnet run
```

The configured development URLs are:

- HTTP: `http://localhost:5268`
- HTTPS: `https://localhost:7052`

To run with a specific launch profile:

```bash
dotnet run --launch-profile https
```

The first HTTPS run may require trusting the local development certificate:

```bash
dotnet dev-certs https --trust
```

## Build and validate

Build the project:

```bash
dotnet build
```

Run the application without rebuilding:

```bash
dotnet run --no-build
```

## Project structure

```text
CarBooking/
├── Areas/Identity/Pages/       # Identity UI pages
├── Controllers/                # MVC controllers
├── Data/                       # EF Core DbContext and migrations
├── Models/                     # View and domain models
├── Views/                      # Razor MVC views
├── wwwroot/                    # CSS, JavaScript, and static libraries
├── Program.cs                  # Service registration and HTTP pipeline
├── appsettings.json            # Shared application configuration
├── appsettings.Development.json
└── CarBooking.csproj           # Project target framework and dependencies
```

## Authentication

Identity is configured with Entity Framework Core and requires confirmed accounts:

```csharp
options.SignIn.RequireConfirmedAccount = true
```

Account pages are available under `/Identity/Account`, including registration, login, logout, and account management.

## Development notes

- The development environment uses the EF Core migrations endpoint for database errors.
- The production pipeline uses the `/Home/Error` error page and HSTS.
- `bin/` and `obj/` are generated build directories and do not need to be edited.
- Keep database connection strings and credentials outside source control for shared or production environments.
