# ClientReport Manager

[Español](#español) · [English](#english)

---

## Español

### Índice

- [Descripción](#descripción)
- [Objetivo del sistema](#objetivo-del-sistema)
- [Problema que resuelve](#problema-que-resuelve)
- [Tecnologías](#tecnologías)
- [Funcionalidades](#funcionalidades)
- [Arquitectura](#arquitectura)
- [Estructura de carpetas](#estructura-de-carpetas)
- [Capturas](#capturas)
- [Requisitos previos](#requisitos-previos)
- [Configuración del entorno](#configuración-del-entorno)
- [Ejecución](#ejecución)
- [Usuario de prueba](#usuario-de-prueba)
- [Base de datos](#base-de-datos)
- [Decisiones técnicas](#decisiones-técnicas)
- [Checklist de pruebas](#checklist-de-pruebas)
- [Errores comunes](#errores-comunes)
- [Mejoras futuras](#mejoras-futuras)
- [Autor](#autor)

### Descripción

**ClientReport Manager** es una aplicación web administrativa desarrollada con **ASP.NET Core MVC**, **C#**, **Entity Framework Core** y **SQL Server**.

Centraliza la información de clientes, permite administrar registros, consultar datos con filtros, ver métricas en un dashboard y generar reportes exportables a CSV.

Nació como práctica integral de desarrollo web con .NET y aplica: arquitectura MVC, programación orientada a objetos, Entity Framework Core, servicios e interfaces, inyección de dependencias, ViewModels, validaciones, autenticación básica y exportación de datos.

### Objetivo del sistema

Ofrecer un sistema web que permita gestionar clientes y generar reportes administrativos de forma centralizada, reduciendo la dependencia de hojas de cálculo y documentos sueltos. El usuario administrativo puede registrar clientes, consultar información, filtrar, ver métricas, generar reportes y exportar resultados desde una interfaz clara.

### Problema que resuelve

En muchas pymes la información de clientes vive en Excel, correos y documentos compartidos, lo que provoca duplicidad, pérdida de datos, búsquedas lentas, falta de reportes y poca trazabilidad. **ClientReport Manager** propone una base de datos relacional con una interfaz web para consultar y reportar.

### Tecnologías

| Área | Tecnología |
|---|---|
| Backend | ASP.NET Core MVC |
| Lenguaje | C# |
| Base de datos | SQL Server |
| ORM | Entity Framework Core |
| Frontend | HTML, CSS, JavaScript |
| Framework UI | Bootstrap |
| Autenticación | Cookies de ASP.NET Core |
| Contraseñas | `PasswordHasher` |
| IDE recomendado | Visual Studio 2022 |
| Exportación | CSV compatible con Excel |

### Funcionalidades

**Autenticación**

- Inicio y cierre de sesión.
- Cookies de autenticación y controladores protegidos con `[Authorize]`.
- Redirección automática al login si no hay sesión.
- Contraseñas gestionadas con `PasswordHasher`.

**Gestión de clientes**

- Registrar, editar y consultar el detalle de un cliente.
- Buscar por texto y filtrar por estado y por tipo.
- Validar identificación duplicada.
- **Desactivación lógica**: el registro se conserva para reportes y trazabilidad.

**Dashboard**

- Total de clientes, activos, inactivos y potenciales.
- Clientes registrados en el mes actual.
- Últimos clientes registrados.
- Resumen por estado y por tipo.

**Reportes administrativos**

- Filtros combinados: fecha de inicio, fecha final, estado, tipo y búsqueda por texto.
- Total de clientes encontrados, resúmenes y tabla detallada.
- Exportación CSV.

**Exportación CSV**

- Respeta los filtros aplicados.
- Separador `;` para mejor compatibilidad con Excel en español.
- Escapa comillas, puntos y coma y saltos de línea.
- Se genera al vuelo, sin guardar el archivo en el servidor.

**Experiencia de usuario**

Sidebar, topbar con el usuario autenticado, tarjetas de métricas, tablas limpias, formularios ordenados, badges de estado, mensajes de éxito, confirmaciones y diseño responsive básico.

### Arquitectura

```text
Controller → Service → DbContext → Database
View       ← ViewModel ← Service
```

| Capa | Responsabilidad |
|---|---|
| Controllers | Reciben solicitudes y devuelven vistas |
| Services | Lógica de negocio y consultas |
| Models | Entidades del sistema |
| ViewModels | Datos específicos para las vistas |
| Data | `DbContext` y configuración de base de datos |
| Views | Interfaz de usuario |
| wwwroot | CSS, JavaScript e imágenes |

### Estructura de carpetas

```text
ClientReportManager/
├── Controllers/
│   ├── AccountController.cs
│   ├── ClientesController.cs
│   ├── DashboardController.cs
│   └── ReportesController.cs
├── Data/
│   └── ApplicationDbContext.cs
├── Models/
│   ├── Cliente.cs
│   ├── EstadoCliente.cs
│   ├── TipoCliente.cs
│   └── Usuario.cs
├── Services/
│   ├── IClienteService.cs / ClienteService.cs
│   ├── IDashboardService.cs / DashboardService.cs
│   ├── IReporteService.cs / ReporteService.cs
│   └── IUsuarioService.cs / UsuarioService.cs
├── ViewModels/
│   ├── ClienteFiltroViewModel.cs
│   ├── DashboardViewModel.cs
│   ├── LoginViewModel.cs
│   └── ReporteClientesViewModel.cs
├── Views/
│   ├── Account/  Clientes/  Dashboard/  Reportes/  Shared/
├── wwwroot/
│   ├── css/  js/  lib/
├── docs/screenshots/
├── appsettings.json
├── Program.cs
├── ClientReportManager.csproj
├── .gitignore
└── README.md
```

### Capturas

Las capturas viven en `docs/screenshots/`:

```text
01-login.png          05-editar-cliente.png
02-dashboard.png      06-reportes.png
03-clientes.png       07-exportacion-csv.png
04-nuevo-cliente.png
```

![Login](docs/screenshots/01-login.png)
![Dashboard](docs/screenshots/02-dashboard.png)
![Clientes](docs/screenshots/03-clientes.png)
![Nuevo cliente](docs/screenshots/04-nuevo-cliente.png)
![Editar cliente](docs/screenshots/05-editar-cliente.png)
![Reportes](docs/screenshots/06-reportes.png)
![Exportación CSV](docs/screenshots/07-exportacion-csv.png)

### Requisitos previos

- Visual Studio 2022.
- **.NET 8 SDK**.
- SQL Server (y opcionalmente SQL Server Management Studio).
- Git.
- Herramientas de Entity Framework Core.

### Configuración del entorno

**1. Clonar y entrar**

```bash
git clone https://github.com/alberto2005-coder/ClientReportManager.git
cd ClientReportManager
```

**2. Restaurar paquetes**

```bash
dotnet restore
```

**3. Revisar la cadena de conexión** en `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=ClientReportManagerDb;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

Con SQL Server Express:

```json
"DefaultConnection": "Server=localhost\\SQLEXPRESS;Database=ClientReportManagerDb;Trusted_Connection=True;TrustServerCertificate=True;"
```

Con usuario SQL:

```json
"DefaultConnection": "Server=localhost;Database=ClientReportManagerDb;User Id=sa;Password=TuClave;TrustServerCertificate=True;"
```

> No subas credenciales reales al repositorio.

**4. Instalar las herramientas de EF Core**

```bash
dotnet tool install --global dotnet-ef
# o, si ya están instaladas
dotnet tool update --global dotnet-ef
dotnet ef
```

**5. Aplicar migraciones**

```bash
dotnet ef database update
```

Esto crea `ClientReportManagerDb` y sus tablas.

**6. Compilar**

```bash
dotnet build
```

**7. Ejecutar**

```bash
dotnet run
```

Abre la URL que muestre la consola (`https://localhost:5001` o `http://localhost:5000`).

### Ejecución desde Visual Studio 2022

1. **Open a project or solution** → `ClientReportManager.sln`.
2. Verifica la cadena de conexión en `appsettings.json`.
3. **Tools → NuGet Package Manager → Package Manager Console**.
4. Ejecuta `Update-Database`.
5. Arranca con `Ctrl + F5`.

### Usuario de prueba

```text
Usuario: admin
Contraseña: 123456
```

Tras el primer inicio de sesión, la contraseña se guarda en formato hash con `PasswordHasher`.

> Cambia estas credenciales antes de desplegar en cualquier entorno real.

### Base de datos

SQL Server con Entity Framework Core en enfoque **Code First**. Tablas principales: `Clientes`, `EstadosCliente`, `TiposCliente` y `Usuarios`.

**Clientes**: `IdCliente`, `NombreCompleto`, `Identificacion`, `Correo`, `Telefono`, `Empresa`, `IdTipoCliente`, `IdEstadoCliente`, `FechaRegistro`, `Observaciones`.

**EstadosCliente**: Activo, Inactivo, Potencial.

**TiposCliente**: Individual, Empresa, Proveedor, Corporativo.

**Usuarios**: usuario inicial `admin / 123456`.

### Decisiones técnicas

- **MVC** para separar modelo, vista y controlador.
- **Servicios** (`ClienteService`, `DashboardService`, `ReporteService`, `UsuarioService`) para que los controladores no acumulen lógica.
- **Interfaces** para desacoplar controladores de implementaciones:

  ```csharp
  public interface IClienteService
  {
      Task<ClienteFiltroViewModel> ObtenerClientesAsync(
          string? buscar, int? idEstadoCliente, int? idTipoCliente);
  }
  ```

- **Inyección de dependencias** registrada en `Program.cs`:

  ```csharp
  builder.Services.AddScoped<IClienteService, ClienteService>();
  builder.Services.AddScoped<IDashboardService, DashboardService>();
  builder.Services.AddScoped<IReporteService, ReporteService>();
  builder.Services.AddScoped<IUsuarioService, UsuarioService>();
  ```

- **Desactivación lógica** en lugar de borrado físico.
- **Fecha final exclusiva** en los reportes:

  ```csharp
  var finExclusivo = fechaFin.Value.Date.AddDays(1);
  query = query.Where(c => c.FechaRegistro < finExclusivo);
  ```

- **CSV con `;`** y escape de comillas, separadores y saltos de línea.
- **Cookies + `[Authorize]`** y `PasswordHasher` para no guardar contraseñas en claro.

### Checklist de pruebas

| Área | Prueba | Resultado esperado |
|---|---|---|
| Auth | Login correcto | Acceso al dashboard |
| Auth | Contraseña incorrecta | Mensaje genérico de error |
| Auth | `/Clientes` sin sesión | Redirección al login |
| Auth | Cerrar sesión | Redirección al login |
| Clientes | Registro válido | Cliente guardado |
| Clientes | Sin nombre / sin identificación | Validación visible |
| Clientes | Identificación duplicada | Error |
| Clientes | Editar / desactivar | Datos actualizados / estado inactivo |
| Clientes | Buscar y filtrar | Coincidencias correctas |
| Dashboard | Contadores | Coinciden con la base de datos |
| Dashboard | Últimos clientes | Muestra los recientes |
| Reportes | Sin filtros | Todos los clientes |
| Reportes | Filtros de fecha, estado y tipo | Respeta la selección |
| Reportes | Fecha inicial > fecha final | Error |
| Reportes | Exportar CSV (con y sin filtros) | Descarga correcta |

### Errores comunes

- **No conecta a SQL Server**: comprueba que el servicio esté iniciado y la cadena de conexión sea correcta.
- **Fallo al migrar**: instala `dotnet-ef` y verifica que `dotnet build` pase.
- **Aviso de certificado HTTPS**: `dotnet dev-certs https --trust`.
- **No aparece la base de datos**: vuelve a ejecutar `dotnet ef database update`.
- **`admin` no entra**: confirma que la migración insertó el usuario y revisa la tabla `Usuarios`.

### Mejoras futuras

Roles y permisos, auditoría de acciones, paginación y ordenamiento en tablas, exportación a Excel y PDF, gráficos con Chart.js, pruebas unitarias, logs de errores, despliegue en Azure App Service, API REST, separación frontend/backend, recuperación de contraseña, migración a ASP.NET Core Identity, modo oscuro e historial de cambios por cliente.

### Autor

Práctica profesional de desarrollo web con .NET centrada en sistemas administrativos, reportes, seguridad básica, arquitectura MVC y experiencia de usuario. Proyecto funcional en versión inicial: login, dashboard, gestión de clientes, reportes, exportación CSV, validaciones e interfaz administrativa.

---

## English

### Table of contents

- [Overview](#overview)
- [Goal](#goal)
- [Problem it solves](#problem-it-solves)
- [Tech stack](#tech-stack)
- [Features](#features)
- [Architecture](#architecture)
- [Folder structure](#folder-structure)
- [Screenshots](#screenshots)
- [Prerequisites](#prerequisites)
- [Local setup](#local-setup)
- [Running](#running)
- [Test user](#test-user)
- [Database](#database)
- [Technical decisions](#technical-decisions)
- [Test checklist](#test-checklist)
- [Common errors](#common-errors)
- [Future improvements](#future-improvements)
- [Author](#author)

### Overview

**ClientReport Manager** is an administrative web app built with **ASP.NET Core MVC**, **C#**, **Entity Framework Core** and **SQL Server**.

It centralizes client information, lets you manage records, query data with filters, view metrics on a dashboard and generate reports exportable to CSV.

It started as an end-to-end .NET web development exercise and applies: MVC architecture, object-oriented programming, Entity Framework Core, services and interfaces, dependency injection, ViewModels, validation, basic authentication and data export.

### Goal

Provide a web system to manage clients and produce administrative reports in a central place, reducing reliance on spreadsheets and scattered documents. An administrative user can register clients, look up information, filter, read metrics, generate reports and export results from a clear interface.

### Problem it solves

In many small and mid-sized companies, client data lives in Excel, email and shared documents, causing duplicates, lost records, slow searches, missing reports and poor traceability. **ClientReport Manager** proposes a relational database behind a web interface for querying and reporting.

### Tech stack

| Area | Technology |
|---|---|
| Backend | ASP.NET Core MVC |
| Language | C# |
| Database | SQL Server |
| ORM | Entity Framework Core |
| Frontend | HTML, CSS, JavaScript |
| UI framework | Bootstrap |
| Authentication | ASP.NET Core cookies |
| Passwords | `PasswordHasher` |
| Recommended IDE | Visual Studio 2022 |
| Export | Excel-friendly CSV |

### Features

**Authentication**

- Sign in and sign out.
- Cookie authentication and `[Authorize]`-protected controllers.
- Automatic redirect to login when there is no session.
- Passwords handled with `PasswordHasher`.

**Client management**

- Create, edit and inspect a client's details.
- Text search plus filters by status and type.
- Duplicate identification validation.
- **Soft delete**: records are kept for reporting and traceability.

**Dashboard**

- Total, active, inactive and prospective clients.
- Clients registered in the current month.
- Most recent clients.
- Breakdown by status and by type.

**Administrative reports**

- Combined filters: start date, end date, status, type and text search.
- Total matches, summaries and a detailed table.
- CSV export.

**CSV export**

- Honors the filters the user applied.
- Uses `;` as separator for better Spanish-locale Excel compatibility.
- Escapes quotes, semicolons and line breaks.
- Streamed directly, never written to the server disk.

**User experience**

Sidebar, topbar with the signed-in user, metric cards, clean tables, tidy forms, status badges, success messages, confirmations and a basic responsive layout.

### Architecture

```text
Controller → Service → DbContext → Database
View       ← ViewModel ← Service
```

| Layer | Responsibility |
|---|---|
| Controllers | Take requests and return views |
| Services | Business logic and main queries |
| Models | System entities |
| ViewModels | View-specific data |
| Data | `DbContext` and database configuration |
| Views | User interface |
| wwwroot | CSS, JavaScript and images |

### Folder structure

```text
ClientReportManager/
├── Controllers/
│   ├── AccountController.cs
│   ├── ClientesController.cs
│   ├── DashboardController.cs
│   └── ReportesController.cs
├── Data/
│   └── ApplicationDbContext.cs
├── Models/
│   ├── Cliente.cs
│   ├── EstadoCliente.cs
│   ├── TipoCliente.cs
│   └── Usuario.cs
├── Services/
│   ├── IClienteService.cs / ClienteService.cs
│   ├── IDashboardService.cs / DashboardService.cs
│   ├── IReporteService.cs / ReporteService.cs
│   └── IUsuarioService.cs / UsuarioService.cs
├── ViewModels/
│   ├── ClienteFiltroViewModel.cs
│   ├── DashboardViewModel.cs
│   ├── LoginViewModel.cs
│   └── ReporteClientesViewModel.cs
├── Views/
│   ├── Account/  Clientes/  Dashboard/  Reportes/  Shared/
├── wwwroot/
│   ├── css/  js/  lib/
├── docs/screenshots/
├── appsettings.json
├── Program.cs
├── ClientReportManager.csproj
├── .gitignore
└── README.md
```

### Screenshots

Screenshots live in `docs/screenshots/`:

```text
01-login.png          05-editar-cliente.png
02-dashboard.png      06-reportes.png
03-clientes.png       07-exportacion-csv.png
04-nuevo-cliente.png
```

![Login](docs/screenshots/01-login.png)
![Dashboard](docs/screenshots/02-dashboard.png)
![Clients](docs/screenshots/03-clientes.png)
![New client](docs/screenshots/04-nuevo-cliente.png)
![Edit client](docs/screenshots/05-editar-cliente.png)
![Reports](docs/screenshots/06-reportes.png)
![CSV export](docs/screenshots/07-exportacion-csv.png)

### Prerequisites

- Visual Studio 2022.
- **.NET 8 SDK**.
- SQL Server (and optionally SQL Server Management Studio).
- Git.
- Entity Framework Core tools.

### Local setup

**1. Clone and enter**

```bash
git clone https://github.com/alberto2005-coder/ClientReportManager.git
cd ClientReportManager
```

**2. Restore packages**

```bash
dotnet restore
```

**3. Check the connection string** in `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=ClientReportManagerDb;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

With SQL Server Express:

```json
"DefaultConnection": "Server=localhost\\SQLEXPRESS;Database=ClientReportManagerDb;Trusted_Connection=True;TrustServerCertificate=True;"
```

With a SQL user:

```json
"DefaultConnection": "Server=localhost;Database=ClientReportManagerDb;User Id=sa;Password=YourPassword;TrustServerCertificate=True;"
```

> Never commit real credentials.

**4. Install the EF Core tools**

```bash
dotnet tool install --global dotnet-ef
# or, if already installed
dotnet tool update --global dotnet-ef
dotnet ef
```

**5. Apply migrations**

```bash
dotnet ef database update
```

This creates `ClientReportManagerDb` and its tables.

**6. Build**

```bash
dotnet build
```

**7. Run**

```bash
dotnet run
```

Open the URL printed by the console (`https://localhost:5001` or `http://localhost:5000`).

### Running from Visual Studio 2022

1. **Open a project or solution** → `ClientReportManager.sln`.
2. Verify the connection string in `appsettings.json`.
3. **Tools → NuGet Package Manager → Package Manager Console**.
4. Run `Update-Database`.
5. Start with `Ctrl + F5`.

### Test user

```text
Username: admin
Password: 123456
```

After the first sign-in, the password is stored as a hash via `PasswordHasher`.

> Change these credentials before deploying to any real environment.

### Database

SQL Server with Entity Framework Core using a **Code First** approach. Main tables: `Clientes`, `EstadosCliente`, `TiposCliente` and `Usuarios`.

**Clientes**: `IdCliente`, `NombreCompleto`, `Identificacion`, `Correo`, `Telefono`, `Empresa`, `IdTipoCliente`, `IdEstadoCliente`, `FechaRegistro`, `Observaciones`.

**EstadosCliente**: Activo, Inactivo, Potencial.

**TiposCliente**: Individual, Empresa, Proveedor, Corporativo.

**Usuarios**: seed user `admin / 123456`.

### Technical decisions

- **MVC** to separate model, view and controller.
- **Services** (`ClienteService`, `DashboardService`, `ReporteService`, `UsuarioService`) so controllers stay thin.
- **Interfaces** to decouple controllers from implementations:

  ```csharp
  public interface IClienteService
  {
      Task<ClienteFiltroViewModel> ObtenerClientesAsync(
          string? buscar, int? idEstadoCliente, int? idTipoCliente);
  }
  ```

- **Dependency injection** registered in `Program.cs`:

  ```csharp
  builder.Services.AddScoped<IClienteService, ClienteService>();
  builder.Services.AddScoped<IDashboardService, DashboardService>();
  builder.Services.AddScoped<IReporteService, ReporteService>();
  builder.Services.AddScoped<IUsuarioService, UsuarioService>();
  ```

- **Soft delete** instead of physical deletion.
- **Exclusive end date** in reports:

  ```csharp
  var endExclusive = endDate.Value.Date.AddDays(1);
  query = query.Where(c => c.FechaRegistro < endExclusive);
  ```

- **CSV with `;`** and escaping of quotes, separators and line breaks.
- **Cookies + `[Authorize]`** and `PasswordHasher` so no plaintext passwords are stored.

### Test checklist

| Area | Test | Expected result |
|---|---|---|
| Auth | Correct login | Dashboard access |
| Auth | Wrong password | Generic error message |
| Auth | `/Clientes` without a session | Redirect to login |
| Auth | Sign out | Redirect to login |
| Clients | Valid registration | Client saved |
| Clients | Missing name / identification | Validation shown |
| Clients | Duplicate identification | Error |
| Clients | Edit / deactivate | Data updated / status inactive |
| Clients | Search and filter | Correct matches |
| Dashboard | Counters | Match the database |
| Dashboard | Latest clients | Shows recent ones |
| Reports | No filters | All clients |
| Reports | Date, status and type filters | Selection honored |
| Reports | Start date > end date | Error |
| Reports | Export CSV (with and without filters) | Correct download |

### Common errors

- **Cannot connect to SQL Server**: check the service is running and the connection string is correct.
- **Migration failure**: install `dotnet-ef` and make sure `dotnet build` succeeds.
- **HTTPS certificate warning**: `dotnet dev-certs https --trust`.
- **Database not created**: re-run `dotnet ef database update`.
- **`admin` cannot sign in**: confirm the migration seeded the user and inspect the `Usuarios` table.

### Future improvements

Roles and permissions, action auditing, pagination and dynamic sorting, real Excel and PDF export, Chart.js charts, unit tests, error logging, Azure App Service deployment, a REST API, frontend/backend split, password recovery, migration to ASP.NET Core Identity, dark mode and per-client change history.

### Author

A professional .NET web development exercise focused on administrative systems, reporting, basic security, MVC architecture and user experience. Functional initial version: login, dashboard, client management, reports, CSV export, validations and an administrative UI.
