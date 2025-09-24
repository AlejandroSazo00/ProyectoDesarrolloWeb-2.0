# 🏀 Basketball Scoreboard - Documentación Completa
## Manuel Alejandro Sazo Linares
## 7690-20-13585
## msazol1@miumg.edu.gt

## 📋 Tabla de Contenidos
- [Descripción del Proyecto](#descripción-del-proyecto)
- [Arquitectura del Sistema](#arquitectura-del-sistema)
- [Tecnologías Utilizadas](#tecnologías-utilizadas)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Funcionalidades Implementadas](#funcionalidades-implementadas)
- [Base de Datos](#base-de-datos)
- [API Endpoints](#api-endpoints)
- [Instalación y Configuración](#instalación-y-configuración)
- [Uso de la Aplicación](#uso-de-la-aplicación)
- [Pruebas y Validación](#pruebas-y-validación)
- [Resolución de Problemas](#resolución-de-problemas)

## 📖 Descripción del Proyecto

Sistema completo de marcador de baloncesto desarrollado con Angular (frontend) y .NET 8 (backend), con persistencia en SQL Server. La aplicación permite gestionar partidos de baloncesto con funcionalidades completas de marcador, temporizador, sistema de cuartos y registro de faltas.

### 🎯 Objetivos Cumplidos
- ✅ Marcador de puntos para equipos Local y Visitante
- ✅ Botones para sumar/restar puntos (+1, +2, +3, -1)
- ✅ Sistema de temporizador configurable (10 minutos por defecto)
- ✅ Gestión de cuartos (1-4) con avance manual y automático
- ✅ Contador de faltas por equipo
- ✅ Persistencia completa en base de datos SQL Server
- ✅ Interfaz responsiva y moderna
- ✅ Notificaciones visuales del temporizador

## 🏗️ Arquitectura del Sistema

```
┌─────────────────┐    HTTP/REST    ┌─────────────────┐    Entity Framework    ┌─────────────────┐
│                 │ ◄──────────────► │                 │ ◄─────────────────────► │                 │
│  Angular Client │                 │  .NET 8 Web API │                        │  SQL Server DB  │
│  (Frontend)     │                 │   (Backend)     │                        │                 │
│  Port: 4200     │                 │   Port: 5163    │                        │ BasketballDB    │
└─────────────────┘                 └─────────────────┘                        └─────────────────┘
```

### 🔧 Componentes Principales

#### Frontend (Angular)
- **ScoreboardComponent**: Gestión de puntajes y faltas
- **TimerComponent**: Temporizador con notificaciones visuales
- **GameService**: Lógica de negocio y comunicación con API
- **ApiService**: Servicios HTTP para comunicación con backend

#### Backend (.NET 8)
- **PartidosController**: API REST para gestión de partidos
- **FaltasController**: API REST para gestión de faltas
- **BasketballDbContext**: Contexto de Entity Framework
- **Models**: Entidades Partido y Falta

## 💻 Tecnologías Utilizadas

### Frontend
- **Angular 17**: Framework principal
- **TypeScript**: Lenguaje de programación
- **RxJS**: Programación reactiva
- **SCSS**: Estilos avanzados
- **Bootstrap**: Framework CSS para diseño responsivo

### Backend
- **.NET 8**: Framework backend
- **ASP.NET Core Web API**: API REST
- **Entity Framework Core**: ORM
- **SQL Server**: Base de datos

### Herramientas de Desarrollo
- **Visual Studio Code**: Editor principal
- **SQL Server Management Studio**: Gestión de base de datos
- **Angular CLI**: Herramientas de Angular
- **dotnet CLI**: Herramientas de .NET

## 📁 Estructura del Proyecto

```
Proyecto01/
├── basketball-scoreboard/          # Frontend Angular
│   ├── src/
│   │   ├── app/
│   │   │   ├── scoreboard/         # Componente marcador
│   │   │   ├── timer/              # Componente temporizador
│   │   │   └── services/           # Servicios Angular
│   │   └── assets/                 # Recursos estáticos
│   ├── angular.json
│   ├── package.json
│   └── tsconfig.json
├── BasketballScoreboardAPI/        # Backend .NET
│   ├── Controllers/                # Controladores API
│   ├── Models/                     # Entidades del dominio
│   ├── Data/                       # Contexto de base de datos
│   ├── Migrations/                 # Migraciones EF
│   └── Program.cs                  # Configuración principal
└── README.md                       # Documentacion de Markdown
```

## ⚡ Funcionalidades Implementadas

### 🏀 Sistema de Marcador
- **Equipos**: Local vs Visitante
- **Puntajes**: Visualización en tiempo real
- **Botones de puntuación**:
  - `+1`: Tiro libre o canasta simple
  - `+2`: Canasta de campo
  - `+3`: Canasta de tres puntos
  - `-1`: Corrección de puntaje
- **Protección**: Los puntajes no pueden ser negativos

### ⏱️ Sistema de Temporizador
- **Duración**: 10 minutos por cuarto (configurable)
- **Controles**: Iniciar, Pausar, Reiniciar
- **Estados visuales**:
  - Normal: Texto blanco
  - Advertencia: Texto naranja (< 1 minuto)
  - Tiempo agotado: Texto rojo parpadeante
- **Persistencia**: Se guarda el tiempo restante en base de datos

### 🔢 Sistema de Cuartos
- **Cuartos**: 1, 2, 3, 4
- **Avance automático**: Al terminar el tiempo
- **Avance manual**: Botón "Siguiente Cuarto"
- **Lógica especial**: Cada cuarto crea un registro separado en BD
- **Reinicio**: Puntajes y faltas se resetean entre cuartos

### ⚠️ Sistema de Faltas
- **Contador por equipo**: Local y Visitante
- **Botón +Falta**: Incrementa faltas del equipo
- **Persistencia**: Vinculadas al partido actual en BD

### 🔄 Sistema de Gestión de Partidos
- **Nuevo Juego**: Reinicia todo a estado inicial
- **Lógica especial Cuarto 4**: Los datos finales se preservan al crear nuevo juego
- **Otros cuartos**: Se resetean sin guardar al crear nuevo juego

## 🗄️ Base de Datos

### 📊 Estructura de Tablas

#### Tabla: `Partidos`
```sql
CREATE TABLE Partidos (
    Id int IDENTITY(1,1) PRIMARY KEY,
    EquipoLocal nvarchar(50) NOT NULL,
    PuntosLocal int NOT NULL DEFAULT 0,
    EquipoVisitante nvarchar(50) NOT NULL,
    PuntosVisitante int NOT NULL DEFAULT 0,
    CuartoActual int NOT NULL DEFAULT 1,
    TiempoRestante int NOT NULL DEFAULT 600,
    Fecha datetime2 NOT NULL DEFAULT GETDATE()
);
```

#### Tabla: `Faltas`
```sql
CREATE TABLE Faltas (
    Id int IDENTITY(1,1) PRIMARY KEY,
    PartidoId int NOT NULL,
    Equipo nvarchar(50) NOT NULL,
    Faltas int NOT NULL DEFAULT 0,
    FOREIGN KEY (PartidoId) REFERENCES Partidos(Id)
);
```

### 🔗 Relaciones
- **Uno a Muchos**: Un Partido puede tener múltiples registros de Faltas
- **Clave Foránea**: `Faltas.PartidoId` → `Partidos.Id`

### 📈 Lógica de Datos
- **Por cuarto**: Cada cuarto genera un registro separado en `Partidos`
- **Continuidad**: Los IDs se incrementan automáticamente
- **Preservación**: Los datos del Cuarto 4 se mantienen al crear nuevo juego

## 🌐 API Endpoints

### 📋 Partidos Controller

#### `POST /api/partidos`
Crea un nuevo partido
```json
Request:
{
    "equipoLocal": "Local",
    "equipoVisitante": "Visitante"
}

Response:
{
    "id": 1,
    "equipoLocal": "Local",
    "puntosLocal": 0,
    "equipoVisitante": "Visitante",
    "puntosVisitante": 0,
    "cuartoActual": 1,
    "tiempoRestante": 600,
    "fecha": "2025-08-22T17:00:00"
}
```

#### `PUT /api/partidos/{id}`
Actualiza un partido existente
```json
Request:
{
    "puntosLocal": 5,
    "puntosVisitante": 3,
    "cuartoActual": 1,
    "tiempoRestante": 580
}

Response: 204 No Content
```

#### `GET /api/partidos/{id}`
Obtiene un partido específico
```json
Response:
{
    "id": 1,
    "equipoLocal": "Local",
    "puntosLocal": 5,
    "equipoVisitante": "Visitante",
    "puntosVisitante": 3,
    "cuartoActual": 1,
    "tiempoRestante": 580,
    "fecha": "2025-08-22T17:00:00",
    "faltas": [...]
}
```

#### `GET /api/partidos`
Obtiene todos los partidos
```json
Response: [
    {
        "id": 1,
        "equipoLocal": "Local",
        "puntosLocal": 5,
        "equipoVisitante": "Visitante",
        "puntosVisitante": 3,
        "cuartoActual": 1,
        "tiempoRestante": 580,
        "fecha": "2025-08-22T17:00:00"
    }
]
```

### ⚠️ Faltas Controller

#### `PUT /api/faltas`
Actualiza las faltas de un equipo
```json
Request:
{
    "partidoId": 1,
    "equipo": "Local",
    "faltas": 3
}

Response: 204 No Content
```

## 🚀 Instalación y Configuración

### 📋 Prerrequisitos
- **Node.js** (v18 o superior)
- **Angular CLI** (`npm install -g @angular/cli`)
- **.NET 8 SDK**
- **SQL Server** (LocalDB o instancia completa)
- **Visual Studio Code** (recomendado)

### 🔧 Configuración del Backend

1. **Navegar al directorio del backend**:
```bash
cd BasketballScoreboardAPI
```

2. **Restaurar paquetes NuGet**:
```bash
dotnet restore
```

3. **Configurar cadena de conexión** en `appsettings.json`:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=LAPTOP-KELEBF4U;Database=BasketballScoreboardDB;Trusted_Connection=true;TrustServerCertificate=true;"
  }
}
```

4. **Aplicar migraciones**:
```bash
dotnet ef database update
```

5. **Ejecutar el backend**:
```bash
dotnet run
```
> El backend estará disponible en: http://localhost:5163

### 🎨 Configuración del Frontend

1. **Navegar al directorio del frontend**:
```bash
cd basketball-scoreboard
```

2. **Instalar dependencias**:
```bash
npm install
```

3. **Ejecutar el frontend**:
```bash
ng serve
```
> El frontend estará disponible en: http://localhost:4200

## 🎮 Uso de la Aplicación

### 🏁 Inicio de Partida
1. Abrir http://localhost:4200
2. Hacer clic en **"Nuevo Juego"**
3. La aplicación inicializa:
   - Puntajes: 0-0
   - Cuarto: 1
   - Timer: 10:00
   - Faltas: 0-0

### 📊 Gestión de Puntajes
- **Sumar puntos**: Clic en botones `+1`, `+2`, `+3`
- **Restar puntos**: Clic en botón `-1`
- **Actualización**: Los cambios se reflejan inmediatamente
- **Persistencia**: Se guardan automáticamente en BD

### ⏰ Control del Temporizador
- **Iniciar**: Botón "Iniciar"
- **Pausar**: Botón "Pausar"
- **Reiniciar**: Botón "Reiniciar"
- **Estados visuales**:
  - Normal: Blanco
  - < 1 min: Naranja
  - Tiempo agotado: Rojo parpadeante

### 🔢 Avance de Cuartos
- **Manual**: Botón "Siguiente Cuarto"
- **Automático**: Al terminar el tiempo
- **Efecto**: Crea nuevo registro en BD, resetea puntajes

### ⚠️ Registro de Faltas
- **Añadir falta**: Botón "+Falta" por equipo
- **Visualización**: Contador en tiempo real
- **Persistencia**: Vinculadas al partido actual

## 🧪 Pruebas y Validación

### ✅ Verificación de Funcionalidades

#### Test 1: Creación de Partido
```sql
-- Verificar en BD después de "Nuevo Juego"
USE BasketballScoreboardDB;
SELECT * FROM Partidos ORDER BY Id DESC;
-- Debe mostrar: ID nuevo, Cuarto 1, Puntajes 0-0
```

#### Test 2: Actualización de Puntajes
1. Hacer clic `+2 Local`, `+3 Visitante`
2. Verificar en BD:
```sql
SELECT PuntosLocal, PuntosVisitante FROM Partidos WHERE Id = [último_id];
-- Debe mostrar: PuntosLocal=2, PuntosVisitante=3
```

#### Test 3: Avance de Cuartos
1. Hacer clic "Siguiente Cuarto"
2. Verificar creación de nuevo registro:
```sql
SELECT Id, CuartoActual, PuntosLocal, PuntosVisitante 
FROM Partidos ORDER BY Id DESC LIMIT 2;
-- Debe mostrar: 2 registros, cuartos diferentes
```

#### Test 4: Lógica Cuarto 4
1. Avanzar hasta Cuarto 4
2. Añadir puntos: Local 10, Visitante 8
3. Hacer clic "Nuevo Juego"
4. Verificar preservación:
```sql
SELECT * FROM Partidos WHERE CuartoActual = 4 ORDER BY Id DESC LIMIT 1;
-- Debe mostrar: PuntosLocal=10, PuntosVisitante=8
```

### 🔍 Logs de Depuración
- **Frontend**: Consola del navegador (F12)
- **Backend**: Terminal donde corre `dotnet run`
- **Base de datos**: SQL Server Management Studio

## 🛠️ Resolución de Problemas

### ❌ Problemas Comunes

#### Error: "ERR_CONNECTION_REFUSED"
**Causa**: Backend no está ejecutándose
**Solución**:
```bash
cd BasketballScoreboardAPI
dotnet run
```

#### Error: "ng serve" falla
**Causa**: Dependencias no instaladas
**Solución**:
```bash
cd basketball-scoreboard
npm install
ng serve
```

#### Error: Base de datos no existe
**Causa**: Migraciones no aplicadas
**Solución**:
```bash
cd BasketballScoreboardAPI
dotnet ef database update
```

#### Error: Página se cierra automáticamente
**Causa**: Servidor Angular crasheado
**Solución**:
1. Detener `ng serve` (Ctrl+C)
2. Reiniciar: `ng serve`

### 🔧 Comandos de Mantenimiento

#### Reiniciar Base de Datos
```bash
cd BasketballScoreboardAPI
dotnet ef database drop --force
dotnet ef database update
```

#### Limpiar y Reconstruir Frontend
```bash
cd basketball-scoreboard
rm -rf node_modules
npm install
ng serve
```

#### Verificar Estado de Servidores
```bash
# Backend
curl http://localhost:5163/api/partidos

# Frontend
curl http://localhost:4200
```

## 📈 Métricas del Proyecto

### 📊 Estadísticas de Desarrollo
- **Tiempo total**: ~6 horas
- **Componentes Angular**: 2 principales
- **Servicios**: 2 (GameService, ApiService)
- **Endpoints API**: 5
- **Tablas BD**: 2
- **Funcionalidades**: 100% completadas

### 🎯 Cobertura de Requisitos
- ✅ **Marcador de puntos**: 100%
- ✅ **Gestión de tiempo**: 100%
- ✅ **Sistema de cuartos**: 100%
- ✅ **Contador de faltas**: 100%
- ✅ **Control general**: 100%
- ✅ **Base de datos**: 100%
- ✅ **Interfaz responsiva**: 100%

## 🏆 Conclusiones

El proyecto **Basketball Scoreboard** ha sido completado exitosamente, cumpliendo todos los requisitos especificados. La aplicación ofrece una experiencia completa de gestión de partidos de baloncesto con:

- **Arquitectura robusta**: Separación clara entre frontend y backend
- **Persistencia confiable**: Todos los datos se guardan en SQL Server
- **Interfaz intuitiva**: Diseño responsivo y fácil de usar
- **Funcionalidades completas**: Desde marcador básico hasta gestión avanzada de cuartos
- **Código mantenible**: Estructura clara y documentada

La aplicación está lista para uso en producción y puede ser extendida con funcionalidades adicionales como estadísticas avanzadas, múltiples partidos simultáneos, o integración con sistemas externos.

---

**Desarrollado por**: Equipo de Desarrollo Basketball Scoreboard  
**Fecha**: Agosto 2025  
**Versión**: 1.0.0  
**Estado**: ✅ Completado y Funcional

