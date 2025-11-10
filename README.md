# Taller de Pruebas de Integración y Sistema

## Camilo Otalora
## Julian Pedraza
## Joel Acosta

# Nos toca hacer esto:

### Ejecución de las pruebas

- Solo unitarias:

```bash
mvn test
```

- Unitarias + integración + sistema:

```bash
mvn verify
```

Reporte de cobertura combinado con JaCoCo:

```gherkin
target/site/jacoco/index.html
```

---


## PARA ENTREGAR CON ESTE TALLER

### 1) Repositorio

- **Repositorio Git** con el proyecto completo y **URL pública o acceso por invitación**.
- Archivo **`.gitignore`** (excluir `target/`, `.idea/`, `.vscode/`, etc.).
- Archivo **`integrantes.txt`** o sección en el README con nombres y correos institucionales.
- **Rama principal ejecutable:** debe compilar y correr con `mvn clean verify` sin configuraciones manuales adicionales.

### 2) Documentación en Wiki (obligatoria)

> Toda la documentación del taller se entrega en el **Wiki del repositorio**.
> No se requiere PDF; el Wiki es la entrega oficial.

Estructura mínima sugerida del Wiki:

- **Inicio:** descripción breve del dominio, propósito del sistema y miembros del equipo.
- **Tipos de pruebas:** diferencia clara entre unitarias, integración y sistema (tabla o esquema).
- **Arquitectura limpia:** diagrama de capas usadas (`domain`, `application`, `infrastructure`, `delivery`).
- **Pruebas de Integración:** explicación de cómo se conectan las capas y la base de datos (H2 o mock).
- **Pruebas con Mockito:** ejemplos de uso de `when(...)`, `verify(...)`, `never(...)`.
- **Pruebas de Sistema (HTTP):** escenarios y evidencias de ejecución (capturas o respuestas JSON).
- **Resultados:** capturas del **reporte JaCoCo** y breve análisis de cobertura.
- **Conclusiones técnicas:** aprendizajes y limitaciones detectadas.

Incluye **enlaces al código** (`Registry.java`, `RegistryController.java`, tests) dentro de cada sección del Wiki.

### 3) Pruebas de Integración

- Al menos **3 pruebas con base de datos H2** cubriendo interacciones reales entre `Registry` y `RegistryRepository`.
- Casos mínimos:
  - Persona válida → `VALID`
  - Persona duplicada → `DUPLICATED`
  - Persona menor de edad → `UNDERAGE`
  - Persona fallecida → `DEAD`
- Deben ejecutarse sin mocks, verificando que los datos se persisten realmente.
- Usa formato **AAA (Arrange – Act – Assert)** y nombres descriptivos (`shouldReturnDuplicatedWhenIdExists()`).

### 4) Pruebas de Integración con Mocks

- Al menos **2 pruebas con Mockito**, simulando el repositorio o adaptador externo.
- Verificar interacciones con:
  - `verify(repo).save(...)`
  - `verify(repo, never()).save(...)`
- Incluir un caso de excepción controlada (`when(repo.save(...)).thenThrow(...)`) y manejo correcto del error.
- Comenta brevemente el propósito de cada test y la lógica simulada.

### 5) Pruebas de Sistema (HTTP)

- Al menos **2 pruebas end-to-end** usando:
  - `TestRestTemplate`, `MockMvc` o cliente HTTP equivalente.
- Validar los endpoints reales (`/register`) devolviendo respuestas HTTP correctas (`200`, `400`, `500`).
- Casos mínimos:
  - Registro exitoso (status 200, body “VALID”).
  - Entrada inválida o inconsistente (status 400 / 422).
- Adjuntar en el Wiki **capturas del resultado** (Postman o terminal).

### 6) Cobertura (JaCoCo)

- Reporte **JaCoCo** generado en `target/site/jacoco/index.html`.
- **Cobertura global ≥ 80%**, y al menos **70% en el paquete `application` y `delivery`**.
- Adjuntar capturas en el Wiki e indicar **qué clases no se pudieron cubrir y por qué** (p. ej. excepciones controladas, código legado, etc.).

### 7) Matriz de pruebas de integración

- Tabla con los **casos de integración** probados:
  - **Caso**, **Entrada**, **Resultado esperado**, **Tipo de prueba (H2/Mock/HTTP)**, **Test que lo valida**.

**Ejemplo:**

| Caso | Entrada | Resultado Esperado | Tipo | Test |
|------|----------|--------------------|------|------|
| Persona duplicada | ID=101 existente | `DUPLICATED` | H2 | `shouldReturnDuplicatedWhenExists()` |
| Persona válida | ID=200, edad=25 | `VALID` | HTTP | `shouldRegisterValidPerson()` |

### 8) Gestión de defectos

- Archivo **`defectos.md`** con al menos **1 defecto real o simulado** detectado por pruebas de integración o sistema.
  - **Caso probado**
  - **Resultado esperado vs. obtenido**
  - **Causa probable**
  - **Estado:** Abierto / Cerrado
  - **Evidencia:** fragmento de log o screenshot

### 9) Calidad del código

- Clases sin duplicación ni dependencias cíclicas.
- Constantes reutilizables (`MIN_AGE`, `MAX_AGE`, etc.).
- Nombrado claro y uso correcto de paquetes.
- Control de errores con excepciones específicas y manejo en el controlador HTTP.
- Eliminación de código comentado o redundante.

### 10) Reflexión final (en el Wiki)

- ¿Qué capas fueron más difíciles de probar y por qué?
- ¿Qué beneficios observas en usar mocks frente a H2 o base real?
- ¿Cómo mejorarías el diseño de `RegistryController` o `RegistryRepository` para facilitar las pruebas automáticas?
- ¿Qué aprendiste sobre **integración continua (CI)** al ejecutar tus pruebas con Maven y JaCoCo?


