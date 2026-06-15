# Skill Backend — OdontoGate

## Descripción general

Este documento define las buenas prácticas de desarrollo y arquitectura aplicadas en el backend del proyecto **OdontoGate**, construido con **Java + Spring Boot**.

La validación automática se ejecuta con:

```bash
mvn checkstyle:check
```

El archivo de configuración se encuentra en: `checkstyle.xml`

---

## 1. Arquitectura del Backend

El backend sigue una arquitectura en **capas (Layered Architecture)** basada en el patrón **MVC**, organizada así:

```
src/main/java/com/odontogate/
├── controller/     # Recibe peticiones HTTP y delega al service
├── service/        # Lógica de negocio
├── repository/     # Acceso a datos (JPA/Hibernate)
├── model/          # Entidades de la base de datos
├── dto/            # Objetos de transferencia de datos
└── config/         # Configuración general (seguridad, beans, etc.)
```

### Dependencias principales

| Dependencia | Propósito |
|---|---|
| Spring Boot | Framework principal |
| Spring Web | Exposición de endpoints REST |
| Spring Data JPA | Persistencia con base de datos |
| Hibernate | ORM para mapeo de entidades |
| Checkstyle | Validación estática de código |

---

## 2. Buenas prácticas de desarrollo — Java

### MUST (Obligatorio)

Estas reglas son **obligatorias** y el build falla si no se cumplen.

---

#### ✅ MUST-1 — Nombres que revelan intención *(Clean Code Cap. 2)*

- **Clases** en `PascalCase` → `PacienteService`, `CitaController`
- **Métodos** en `camelCase` → `obtenerCita()`, `registrarPaciente()`
- **Variables y parámetros** en `camelCase` → `nombrePaciente`, `fechaCita`
- **Campos** en `camelCase` → `private String apellido`

```java
// ✅ Correcto
public class PacienteService {
    private String nombreCompleto;
    public Paciente obtenerPorId(Long pacienteId) { ... }
}

// ❌ Incorrecto
public class paciente_service {
    private String NombreCompleto;
    public Paciente ObtenerPorId(Long PacienteId) { ... }
}
```

---

#### ✅ MUST-2 — Sin imports con wildcard *(Clean Code Cap. 2)*

Los imports deben ser explícitos, nunca usar `*`.

```java
// ✅ Correcto
import java.util.List;
import java.util.Optional;

// ❌ Incorrecto
import java.util.*;
```

---

#### ✅ MUST-3 — Sin código comentado *(Clean Code Cap. 4)*

El código que ya no se usa debe **eliminarse**, no comentarse.

```java
// ✅ Correcto
public void guardar(Paciente paciente) {
    repository.save(paciente);
}

// ❌ Incorrecto
public void guardar(Paciente paciente) {
    // repository.saveAll(paciente);
    repository.save(paciente);
}
```

---

#### ✅ MUST-4 — Métodos de máximo 35 líneas *(Clean Code Cap. 3)*

Las funciones deben ser pequeñas y hacer una sola cosa.

> Máximo permitido: **35 líneas** (sin contar líneas vacías)

---

#### ✅ MUST-5 — Máximo 5 parámetros por método *(Clean Code Cap. 3)*

```java
// ✅ Correcto
public void registrar(String nombre, String apellido, String email) { ... }

// ❌ Incorrecto — demasiados parámetros
public void registrar(String nombre, String apellido, String email,
                      String telefono, String direccion, String ciudad) { ... }
```

---

### SHOULD (Recomendado)

Estas reglas son **buenas prácticas** que también se validan automáticamente.

---

#### ✅ SHOULD-1 — Sin imports no utilizados *(Clean Code Cap. 17)*

```java
// ❌ Incorrecto — import que no se usa
import java.util.HashMap; // nunca usado en el archivo
```

---

#### ✅ SHOULD-2 — Llaves siempre presentes

Aunque el bloque tenga una sola línea, siempre usar `{}`.

```java
// ✅ Correcto
if (activo) {
    return paciente;
}

// ❌ Incorrecto
if (activo)
    return paciente;
```

---

#### ✅ SHOULD-3 — Una sola instrucción por línea *(Clean Code — legibilidad)*

```java
// ✅ Correcto
int a = 1;
int b = 2;

// ❌ Incorrecto
int a = 1; int b = 2;
```

---

## 3. Evidencia de ejecución

Para ejecutar la validación:

```bash
mvn checkstyle:check
```

> ✅ El resultado debe mostrar `BUILD SUCCESS` sin errores ni advertencias de Checkstyle.

Pegar aquí la captura de pantalla o log del resultado en verde:

'''
PS C:\Users\boris\Desktop\pruebaproyecto\project-backend\OdontoGate> mvn checkstyle:check
[INFO] Scanning for projects...
[INFO] 
[INFO] -----------------< com.OdontoGate:ArtefactoOdontoGate >-----------------
[INFO] Building  0.0.1-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- checkstyle:3.3.1:check (default-cli) @ ArtefactoOdontoGate ---
[INFO] Comenzando auditoría...
Auditoría concluida.
[INFO] You have 0 Checkstyle violations.
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.442 s
[INFO] Finished at: 2026-06-14T02:12:09-05:00
[INFO] ------------------------------------------------------------------------
PS C:\Users\boris\Desktop\pruebaproyecto\project-backend\OdontoGate> 
---

## 4. Exclusiones

La clase principal de la aplicación está excluida de la validación:

- `OdontoGateApplication.java`

---

*Documento generado para el proyecto OdontoGate — UNAL*
