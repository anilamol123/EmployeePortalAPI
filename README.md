# EmployeePortal — Employee Management REST API

An ASP.NET Core 8 Web API for managing employee records backed by SQL Server. Supports full CRUD operations and includes Swagger UI for interactive API exploration in development.

---

## Features

- **Get all employees** — retrieve the full list of employee records
- **Add an employee** — create a new employee with name, email, phone number, and salary
- **Update an employee** — edit any field on an existing employee by GUID
- **Delete an employee** — remove an employee record by GUID
- **Swagger UI** — interactive API docs auto-generated in development mode

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | ASP.NET Core 8 Web API |
| ORM | Entity Framework Core 8 |
| Database | SQL Server |
| API Docs | Swashbuckle / Swagger |

---

## Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8)
- SQL Server (local or remote) — SQL Server Express works for local development
- [EF Core CLI tools](https://learn.microsoft.com/en-us/ef/core/cli/dotnet):
  ```bash
  dotnet tool install --global dotnet-ef
  ```

---

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd EmpoyeePortal
```

### 2. Configure the database connection

Open `appsettings.json` and update the `DefaultConnection` string to point to your SQL Server instance:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=YOUR_SERVER;Database=EmployeeDB;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

### 3. Apply migrations

```bash
dotnet ef database update
```

This creates the `EmployeeDB` database and the `Employees` table.

### 4. Run the application

```bash
dotnet run
```

### 5. Open Swagger UI

```
https://localhost:<port>/swagger
```

The port is shown in the terminal on startup.

---

## API Endpoints

Base path: `/api/employees`

### Get all employees

```
GET /api/employees
```

**Response**
```json
[
  {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "Alice Smith",
    "email": "alice@example.com",
    "phoneNumber": "555-0100",
    "salary": 75000.00
  }
]
```

---

### Add an employee

```
POST /api/employees
```

**Request body**
```json
{
  "name": "Alice Smith",
  "email": "alice@example.com",
  "phoneNumber": "555-0100",
  "salary": 75000.00
}
```

**Response** — `201 Created`

---

### Update an employee

```
PUT /api/employees/{id}
```

| Parameter | Type | Description |
|---|---|---|
| `id` | GUID | The ID of the employee to update |

**Request body**
```json
{
  "name": "Alice Johnson",
  "email": "alice.j@example.com",
  "phoneNumber": "555-0199",
  "salary": 80000.00
}
```

**Response** — the updated employee object, or `404 Not Found`

---

### Delete an employee

```
DELETE /api/employees/{id}
```

| Parameter | Type | Description |
|---|---|---|
| `id` | GUID | The ID of the employee to delete |

**Response** — `204 No Content`, or `404 Not Found`

---

## Project Structure

```
EmpoyeePortal/
├── Controllers/
│   └── EmployeesController.cs   # GET, POST, PUT, DELETE handlers
├── DTO/
│   ├── EmployeeDto.cs           # DTO for creating an employee
│   └── UpdateEmployeeDto.cs     # DTO for updating an employee
├── Migrations/                  # EF Core migration history
├── ApplicationDbContext.cs      # EF Core DbContext
├── Employee.cs                  # Employee entity
├── appsettings.json             # App config and connection string
└── Program.cs                   # App setup, DI, middleware
```

---

## Running Migrations

To add a new migration after modifying the `Employee` model:

```bash
dotnet ef migrations add <MigrationName>
dotnet ef database update
```

To roll back to a previous migration:

```bash
dotnet ef database update <PreviousMigrationName>
```
