# 🏀 Basketball Scoreboard - Proyecto 2

Sistema completo de gestión de marcador de baloncesto con panel administrativo.

## 📋 Descripción del Proyecto

Este proyecto combina un **marcador de baloncesto en tiempo real** con un **sistema administrativo completo** para gestionar equipos, jugadores y partidos. Desarrollado con **.NET Core Web API** y **Angular**.

## 🚀 Tecnologías Utilizadas

### Backend
- **.NET Core 8.0** - Web API
- **Entity Framework Core** - ORM
- **SQL Server** - Base de datos
- **JWT Authentication** - Seguridad
- **Swagger** - Documentación API

### Frontend
- **Angular 18** - Framework frontend
- **TypeScript** - Lenguaje principal
- **Angular Material** - Componentes UI
- **RxJS** - Programación reactiva

## 🏗️ Arquitectura del Sistema

```
├── BasketballScoreboardAPI/          # Backend .NET Core
│   ├── Controllers/                  # Controladores API
│   ├── Models/                      # Modelos de datos
│   ├── Data/                        # Contexto de base de datos
│   ├── Services/                    # Servicios de negocio
│   └── Migrations/                  # Migraciones EF
│
└── basketball-scoreboard/           # Frontend Angular
    ├── src/app/
    │   ├── admin/                   # Módulo administrativo
    │   ├── auth/                    # Autenticación
    │   ├── scoreboard/              # Marcador principal
    │   ├── services/                # Servicios Angular
    │   └── guards/                  # Guards de ruta
    └── assets/                      # Recursos estáticos
```

## 🎯 Funcionalidades Principales

### 🏀 Marcador en Tiempo Real
- ✅ Marcador visual con puntuación en vivo
- ✅ Timer de cuartos con control completo
- ✅ Sistema de faltas por equipo
- ✅ Efectos de sonido y música de fondo
- ✅ Animaciones y transiciones suaves

### 👨‍💼 Panel Administrativo
- ✅ **Login seguro** con JWT
- ✅ **Dashboard** con estadísticas generales
- ✅ **Gestión de Equipos**: CRUD completo con logos y colores
- ✅ **Gestión de Jugadores**: Registro con posiciones y estadísticas
- ✅ **Gestión de Partidos**: Programación e integración con marcador
- ✅ **Roles y permisos** de usuario

### 🔗 Integración Innovadora
- ✅ **Marcador independiente**: Funciona sin admin para demos
- ✅ **Modo integrado**: Admin puede inicializar partidos oficiales
- ✅ **Sincronización**: Resultados se guardan automáticamente
- ✅ **Historial completo**: Todos los partidos quedan registrados

## 🛠️ Instalación y Configuración

### Prerrequisitos
- **.NET 8.0 SDK**
- **Node.js 18+**
- **Angular CLI**
- **SQL Server** (LocalDB o Express)

### Backend (.NET Core API)

```bash
# Navegar al directorio del backend
cd BasketballScoreboardAPI

# Restaurar paquetes NuGet
dotnet restore

# Aplicar migraciones a la base de datos
dotnet ef database update

# Ejecutar la API
dotnet run --urls http://localhost:5163
```

### Frontend (Angular)

```bash
# Navegar al directorio del frontend
cd basketball-scoreboard

# Instalar dependencias
npm install

# Ejecutar la aplicación
ng serve
```

## 🌐 Acceso a la Aplicación

Una vez ejecutados ambos proyectos:

- **🎮 Marcador Principal**: http://localhost:4200
- **👨‍💼 Panel Admin**: http://localhost:4200/admin
- **📚 API Swagger**: http://localhost:5163/swagger
- **🔧 API Base**: http://localhost:5163/api

## 🔐 Credenciales de Acceso

### Administrador
- **Usuario**: `admin`
- **Contraseña**: `Admin123!`

## 📊 Base de Datos

### Modelos Principales

```sql
-- Equipos
Equipos: Id, Nombre, Ciudad, ColorPrimario, ColorSecundario, LogoUrl, Activo

-- Jugadores  
Jugadores: Id, Nombre, Numero, EquipoId, Posicion, Altura, Peso, FechaNacimiento, Nacionalidad, Activo

-- Partidos
Partidos: Id, EquipoLocal, EquipoVisitante, PuntosLocal, PuntosVisitante, Fecha, CuartoActual, TiempoRestante

-- Usuarios
Usuarios: Id, Username, PasswordHash, Role

-- Faltas
Faltas: Id, PartidoId, Equipo, Cantidad, Timestamp
```

## 🎮 Guía de Uso

### Para Usuarios Generales
1. Accede a http://localhost:4200
2. Usa el marcador libremente para demos
3. Controla puntos, tiempo y faltas
4. Disfruta de la música y efectos

### Para Administradores
1. Login en http://localhost:4200/login
2. Accede al panel administrativo
3. Gestiona equipos, jugadores y partidos
4. Inicia partidos oficiales desde admin
5. Revisa estadísticas en el dashboard

## 🔧 Configuración Avanzada

### Variables de Entorno
```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=BasketballScoreboardDB;Trusted_Connection=true;"
  },
  "JwtSettings": {
    "SecretKey": "tu-clave-secreta-muy-segura-de-al-menos-32-caracteres",
    "Issuer": "BasketballScoreboardAPI",
    "Audience": "BasketballScoreboardClient",
    "ExpiryMinutes": 60
  }
}
```

### CORS Configuration
```csharp
// Program.cs
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAngular", policy =>
    {
        policy.WithOrigins("http://localhost:4200")
              .AllowAnyHeader()
              .AllowAnyMethod();
    });
});
```

## 🧪 Testing

### Backend Testing
```bash
cd BasketballScoreboardAPI
dotnet test
```

### Frontend Testing
```bash
cd basketball-scoreboard
ng test
ng e2e
```

## 📈 Características Técnicas

### Seguridad
- ✅ Autenticación JWT
- ✅ Autorización por roles
- ✅ Validación de datos
- ✅ Sanitización de inputs
- ✅ CORS configurado

### Performance
- ✅ Lazy loading de módulos
- ✅ Interceptors HTTP
- ✅ Observables y async/await
- ✅ Optimización de consultas EF
- ✅ Caching de respuestas

### UX/UI
- ✅ Diseño responsive
- ✅ Animaciones suaves
- ✅ Feedback visual
- ✅ Manejo de errores
- ✅ Loading states

## 🐛 Solución de Problemas

### Error de Conexión a Base de Datos
```bash
# Verificar conexión
dotnet ef database update --verbose

# Recrear base de datos
dotnet ef database drop
dotnet ef database update
```

### Error de CORS
```bash
# Verificar que el backend esté en puerto 5163
# Verificar que el frontend esté en puerto 4200
# Revisar configuración CORS en Program.cs
```

### Error de JWT
```bash
# Verificar que la clave secreta tenga al menos 32 caracteres
# Verificar configuración en appsettings.json
# Limpiar localStorage del navegador
```

## 📝 Changelog

### v2.0.0 - Proyecto 2 Completo
- ✅ Panel administrativo completo
- ✅ CRUD de equipos, jugadores y partidos
- ✅ Integración segura con marcador
- ✅ Dashboard con estadísticas
- ✅ Autenticación JWT robusta

### v1.0.0 - Marcador Base
- ✅ Marcador funcional
- ✅ Timer y cuartos
- ✅ Sistema de faltas
- ✅ Efectos de sonido

## 👥 Contribución

Este proyecto fue desarrollado como parte del **Proyecto 2** del curso de desarrollo web.

## 📄 Licencia

Este proyecto es de uso académico y educativo.

---

## 🎉 ¡Proyecto Completado!

**Basketball Scoreboard** es un sistema completo que combina entretenimiento y gestión profesional, perfecto para demostraciones, eventos deportivos y administración de ligas de baloncesto.

**¡Disfruta del juego!** 🏀
