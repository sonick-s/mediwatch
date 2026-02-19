<h1 align="center">Mediwatch</h1>
Es una mini plataforma clínica diseñada para el monitoreo de signos vitales de pacientes, permitiendo la gestión básica de pacientes, el registro clínico de observaciones y la supervisión médica mediante alertas automáticas por valores fuera de rango.

## Indice
- [Objetivo](#objetivo)
- [Arquitectura General](#arquitectura-general)
- [Enlaces a](#enlaces-a)
- [01-Alcance](#01-alcance)
- [02-Requisitos](#02-requisitos)
- [03-Actores-roles-permisos](#03-actores-roles-permisos)
- [04-Casos-de-Uso](#04-casos-de-uso)


## Objetivo
El presente proyecto tiene como objetivo diseñar y desarrollar una plataforma de monitoreo clínico orientada a entornos de telemedicina, comprendiendo los estándares, requisitos técnicos y buenas prácticas necesarias para la implementación de un sistema seguro, escalable y alineado con normativas del sector salud, aplicando los estandares:

- Estandar FHIR con Google Clud Healthcare API
- Control por roles (RBAC)
- JWT Auth
- Validación DTO
- Auditoría (quién registró qué)

## Arquitectura general

## Enlaces a:
> [!Tip]
> Enlaces a los repositorios de GitHub
> Backend [link](https://github.com/sonick-s/mediwatch-backend.git)
> Frontend [link](https://github.com/sonick-s/mediwatch-frontend.git)

## 01-Alcance
El alcance incluye la gestión de usuarios médicos y administrativos, la administración básica de pacientes, el registro manual de signos vitales, la consulta de historial clínico y la generación de alertas simples ante valores fuera de rangos normales.

## 02-Requisitos
| Requisitos Técnicos (TR) | Requisitos Funcionales (FR) | Requisitos No Funcionales (NFR) |
|--------------------------|-----------------------------|----------------------------------|
| Arquitectura Cloud: El sistema deberá desplegarse en Google Cloud Platform. |  Autenticación de Usuarios: El sistema deberá permitir a los usuarios autenticarse mediante credenciales válidas. | Seguridad: El sistema deberá implementar JWT, hashing bcrypt, RBAC y HTTPS. |
| Backend: El backend deberá implementarse en Java con Sprint, desplegarse en Cloud Run y ejecutarse en Docker. | Gestión de Usuarios: El sistema deberá permitir crear usuarios, asignar roles, editar información básica y desactivar usuarios. | Privacidad de Datos: El sistema deberá proteger la información clínica y personal del paciente. |
| Base de Datos: El sistema deberá utilizar PostgreSQL (Cloud SQL) para datos operativos. | Control de Acceso (RBAC): El sistema deberá restringir el acceso según roles definidos. | Rendimiento: Las operaciones críticas deberán responder en menos de 2 segundos. |
| Datos Clínicos: Los datos clínicos deberán almacenarse en un FHIR Store (Healthcare API). | Registro de Pacientes: El sistema deberá permitir registrar pacientes con datos demográficos básicos. | Disponibilidad: El sistema deberá garantizar alta disponibilidad en Cloud Run. |
| Gestión de Secretos: Las credenciales deberán almacenarse en Secret Manager "Posible cambio". | Gestión Básica de Pacientes: El sistema deberá permitir editar, visualizar y desactivar pacientes. | Integridad de Datos: El sistema deberá prevenir duplicados e inconsistencias. |
| Registro de Logs: Los eventos del sistema deberán registrarse en Cloud Logging. | Registro de Signos Vitales: El sistema deberá permitir registrar FC, SpO2 y PAS/PAD. | Auditoría y Logging: El sistema deberá registrar usuario, operación y timestamp. |
|  | Validación de Datos Clínicos: El sistema deberá validar rangos fisiológicos plausibles. | Escalabilidad: El backend deberá soportar escalado horizontal automático. |
|  | Historial Clínico: El sistema deberá permitir visualizar historial de observaciones por paciente. | Usabilidad: La interfaz deberá ser clara e intuitiva para personal clínico. |
|  | Generación de Alertas: El sistema deberá generar alertas ante valores fuera de rango. |  |
|  | Supervisión Médica: El sistema deberá permitir a médicos supervisores consultar pacientes y observaciones. |  |
|  | Integración FHIR: El sistema deberá sincronizar Patient y Observation mediante Healthcare API. |  |

## 03-Actores-Roles-Permisos
| Actor | Descripción | Responsabilidades | Permisos en el Sistema | Restricciones |
|-------|-------------|-------------------|------------------------|---------------|
| Personal Administrativo / Recepción | Usuario responsable de la gestión administrativa del paciente dentro del sistema. | - Registro de pacientes<br>- Actualización de datos demográficos<br>- Verificación de información básica<br>- Gestión de estado del paciente | - Crear pacientes<br>- Editar pacientes<br>- Visualizar listado de pacientes<br>- Desactivar pacientes | - No registra signos vitales<br>- No accede a historial clínico detallado<br>- No gestiona alertas |
| Enfermera / Personal de Enfermería | Usuario clínico encargado del registro de signos vitales del paciente. | - Toma de signos vitales<br>- Registro de observaciones<br>- Seguimiento rutinario | - Ver pacientes asignados<br>- Registrar signos vitales<br>- Consultar historial básico | - No crea pacientes<br>- No edita datos administrativos<br>- No administra usuarios |
| Doctor Responsable | Profesional médico encargado del control clínico directo del paciente. | - Supervisión clínica<br>- Validación de observaciones<br>- Evaluación de alertas | - Registrar signos vitales<br>- Consultar historial clínico<br>- Visualizar alertas del paciente<br>- Supervisión de evolución clínica | - No administra usuarios<br>- No gestiona configuración del sistema |
| Doctor Supervisor / Médico Encargado | Usuario con rol jerárquico de supervisión clínica transversal. | - Supervisión global<br>- Revisión de observaciones<br>- Control de alertas críticas | - Ver todos los pacientes<br>- Ver todas las observaciones<br>- Visualizar alertas globales<br>- Supervisión clínica transversal | - No gestiona configuración técnica<br>- No administra infraestructura |
| Administrador del Sistema / IT | Usuario técnico responsable de la administración del sistema. | - Gestión de usuarios<br>- Configuración del sistema<br>- Auditoría técnica | - Crear usuarios<br>- Asignar roles<br>- Gestionar permisos<br>- Revisar logs<br>- Configurar parámetros del sistema | - No registra signos vitales<br>- No interviene en decisiones clínicas |

## 04-Casos-de-Uso

 Casos de uso modelados en Plant Uml como Diagramas de Actividad (UML Activity Diagram)
   
CU-01 – Autenticarse en el Sistema

<img src="https://github.com/sonick-s/mediwatch/blob/main/docs/autentication.png" width="300"/>

```plantuml
@startuml
title CU-01 – Autenticarse en el Sistema (Diagrama de Actividad)
start
:Usuario ingresa correo y contraseña;
:Sistema valida credenciales;
if (¿Credenciales válidas?) then (Sí)
    if (¿Usuario activo?) then (Sí)
        :Generar JWT;
        :Redirigir al panel principal;
        stop
    else (No)
        :Mostrar mensaje "Usuario inactivo";
        stop
    endif
else (No)
    :Mostrar mensaje "Credenciales inválidas";
    stop
endif
@enduml
```


CU-02 – Registrar Paciente

<img src="https://github.com/sonick-s/mediwatch/blob/main/docs/registro-patient.png" width="300"/>

```plantuml
@startuml
title CU-02 – Registrar Paciente

|Personal Administrativo / Recepción|
start
:Seleccionar "Nuevo Paciente";
:Ingresar datos demográficos;
:Validar campos;
if (¿Datos completos?) then (No)
  :Mostrar error E1\nDatos incompletos;
  stop
else (Sí)
  if (¿Paciente duplicado?) then (Sí)
    :Mostrar error E2\nPaciente duplicado;
    stop
  else (No)
    :Guardar paciente;
    if (¿Sincronizar Patient FHIR?) then (Sí)
      :Sincronizar Patient FHIR;
    endif
    :Confirmar registro exitoso;
    stop
  endif
endif
@enduml
```


CU-03 – Editar Información del Paciente

<img src="https://github.com/sonick-s/mediwatch/blob/main/docs/edit-patient.png" width="300"/>

```plantuml
@startuml
title CU-03 – Editar Información del Paciente (Diagrama de Actividad)
start
:Usuario selecciona "Editar Paciente";
:Usuario busca paciente por ID o nombre;
:Mostrar formulario con datos actuales;
:Usuario modifica campos necesarios;
:Usuario guarda cambios;
:Sistema valida cambios;
if (¿Cambios válidos?) then (Sí)
    :Actualizar paciente en base de datos;
    :Mostrar mensaje "Paciente actualizado exitosamente";
    stop
else (No)
    :Mostrar mensaje de error;
    stop
endif
@enduml
```


CU-04 – Registrar Signos Vitales

<img src="https://github.com/sonick-s/mediwatch/blob/main/docs/registro-signos-vitales.png" width="300"/> 

```plantuml
@startuml
title CU-04 – Registrar Signos Vitales

|Enfermera / Doctor Responsable|

start
:Seleccionar paciente;
:Ingresar FC, SpO2, PAS/PAD;
:Validar rangos fisiológicos;
if (¿Valores válidos?) then (No)
  :Mostrar error E1\nValores inválidos;
  stop
else (Sí)
  :Guardar observación;
  :Evaluar reglas clínicas;
  if (¿Se requiere alerta?) then (Sí)
    :Generar alerta clínica;
  endif
  :Sincronizar Observation FHIR;
  :Confirmar registro exitoso;
  stop
endif
@enduml
```

CU-05 – Consultar Historial Clínico

<img src="https://github.com/sonick-s/mediwatch/blob/main/docs/historial-clinico.png" width="300"/> 

```plantuml
@startuml
title CU-05 – Consultar Historial Clínico

|Enfermera / Doctor / Supervisor|

start

:Seleccionar paciente;
:Solicitar historial clínico;

|Sistema HomeVitals|
:Recuperar observaciones del paciente;

if (¿Historial disponible?) then (No)
  :Mostrar mensaje\n"No hay registros disponibles";
  stop
else (Sí)
  :Mostrar historial clínico;
  stop
endif
@enduml
```

CU-06 – Visualizar Alertas

<img src="https://github.com/sonick-s/mediwatch/blob/main/docs/alerts.png" width="300"/> 

```plantuml
@startuml
title CU-06 – Visualizar Alertas

|Doctor Responsable / Doctor Supervisor|

start

:Acceder al panel de alertas;

|Sistema HomeVitals|
:Recuperar alertas activas;

if (¿Existen alertas activas?) then (No)
  :Mostrar mensaje\n"No hay alertas activas";
  stop
else (Sí)
  :Mostrar alertas activas;
endif

|Doctor Responsable / Doctor Supervisor|
:Filtrar alertas;
:Revisar estado de alerta;

stop

@enduml
```

CU-07 – Supervisión Global

<img src="https://github.com/sonick-s/mediwatch/blob/main/docs/admin-general.png" width="300"/> 

```plantuml
@startuml
title CU-07 – Supervisión Global

|Doctor Supervisor|

start

:Acceder al panel de supervisión;

|Sistema HomeVitals|
:Recuperar listado de pacientes;
:Recuperar alertas activas;
:Mostrar pacientes y alertas;

|Doctor Supervisor|
:Seleccionar paciente;

|Sistema HomeVitals|
:Recuperar observaciones del paciente;
:Mostrar observaciones;

stop

@enduml
```
