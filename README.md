# CQRS Base Template

Template base para microservicios con Clean Architecture y patrón CQRS (.NET 10).

## Características

- ✅ **Clean Architecture** — Separación clara de capas (API, Application, Domain, Infrastructure)
- ✅ **Patrón CQRS** — Commands y Queries desacopladas con MediatR
- ✅ **Repository Pattern** — UnitOfWork + Genéricos + 27+ métodos
- ✅ **Entity Framework Core** — PostgreSQL con configuración automática
- ✅ **FluentValidation** — Validaciones tipadas
- ✅ **AutoMapper** — Mapeo de objetos automático
- ✅ **Swagger/OpenAPI** — Documentación interactiva
- ✅ **Serilog** — Logging estructurado
- ✅ **Health Checks** — Monitoreo de salud
- ✅ **Docker** — Dockerfile + docker-compose incluidos
- ✅ **Tests** — Unit tests + Integration tests (xUnit)
- ✅ **Soft Delete** + **Auditoría** — CreatedAt, UpdatedAt, IsDeleted automáticos
- ✅ **SOLID + DRY** — Principios aplicados en todo el código

## Setup rápido

```bash
# 1. Renombrar proyecto (opcional)
python rename_project.py MiMicroservicio

# 2. Levantar PostgreSQL
docker-compose up -d

# 3. Restaurar + Compilar
dotnet restore
dotnet build

# 4. Aplicar migraciones
dotnet ef database update --project CqrsBase.Infrastructure.Persistence

# 5. Ejecutar API
dotnet run --project CqrsBase/CqrsBase
```

API disponible en `http://localhost:5000/swagger`

## Estructura

```
CqrsBase.sln
├── CqrsBase/                           # API Layer
│   ├── Controllers/                    # REST endpoints
│   ├── appsettings.*.json              # Config por ambiente
│   └── Program.cs
├── CqrsBase.Application/               # Application Layer
│   ├── Commands/                       # CQRS Commands
│   ├── Queries/                        # CQRS Queries
│   ├── Handlers/                       # Command/Query Handlers
│   ├── DTOs/                           # Data Transfer Objects
│   └── Mappings/                       # AutoMapper profiles
├── CqrsBase.Domain/                    # Domain Layer
│   └── Entities/                       # Entidades de negocio
├── CqrsBase.Infrastructure.Persistence/  # Persistence
│   ├── Contexts/                       # DbContext
│   ├── Configurations/                 # EF Entity Configs
│   └── Repositories/                   # Implementación
├── CqrsBase.Infrastructure.Shared/     # Shared Services
├── CqrsBase.UnitTest/                  # Unit Tests
└── CqrsBase.IntegrationTest/           # Integration Tests
```

## Crear nueva entidad

1. **Entidad** → `Domain/Entities/MiEntidad.cs` (heredar `BaseEntity`)
2. **Configuration** → `Infrastructure.Persistence/Configurations/MiEntidadConfiguration.cs` (usar `DefineColumn` + `SetupSchemaAndName`)
3. **DbSet** → `Contexts/CqrsBaseDbContext.cs`
4. **Migration** → `./add_migration.sh AddMiEntidad`
5. **DTO + Commands + Queries + Handlers** según necesidad

## Comandos útiles

```bash
# Base de datos
dotnet ef migrations add NombreMigracion --project CqrsBase.Infrastructure.Persistence
dotnet ef database update --project CqrsBase.Infrastructure.Persistence

# Tests
dotnet test
dotnet test --collect:"XPlat Code Coverage"

# Docker
docker-compose up -d
docker-compose down
```

## Reglas de desarrollo

Ver `.cursorrules` para guías completas. Resumen:
- Métodos ≤20 líneas (idealmente 5-10)
- Clases ≤1000 líneas (idealmente 200-400)
- Guard clauses obligatorias
- Sin comentarios salvo caso especial
- Async/await en todo I/O
- SOLID + DRY en todo el código

## Stack

- .NET 10.0
- ASP.NET Core Web API
- Entity Framework Core 8.0 + PostgreSQL
- MediatR 12.2.0
- FluentValidation 11.8.1
- AutoMapper 12.0.1
- Serilog 3.1.1
- xUnit 2.6.1

## Próximos pasos

1. Leer `.cursorrules` completamente
2. Renombrar proyecto si es necesario
3. Implementar entidades de dominio
4. Crear commands/queries para casos de uso
5. Configurar validaciones
6. Cubrir con tests
7. Configura auth/autorización si es necesario
