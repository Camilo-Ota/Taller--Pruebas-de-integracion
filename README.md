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


### 11) Rúbrica – Taller de Pruebas de Integración y Sistema

| **Criterios de evaluación** | **Indicadores de cumplimiento** | **Excelente (5 pts)** | **Bueno (4 pts)** | **Necesita mejorar (3.5 pts)** | **Deficiente (2.5 pts)** | **No cumple (0 pts)** |
|-----------------------------|----------------------------------|------------------------|-------------------|-------------------------------|--------------------------|------------------------|
| **Estructura del proyecto y repositorio** | El repositorio está correctamente organizado, con `.gitignore`, ramas compilables y documentación básica. | Estructura limpia, compilable con `mvn clean verify`, incluye `.gitignore` y documentación. | Compila correctamente, estructura clara con mínimos ajustes. | Estructura parcialmente ordenada, requiere ajustes menores. | Errores de compilación o estructura desordenada. | No entrega o el código no ejecuta. |
| **Documentación en Wiki** | Contiene secciones completas (Inicio, tipos de pruebas, resultados, reflexión, etc.) con enlaces al código. | Wiki completo, claro y con enlaces a todas las clases y tests. | Wiki completo con leves omisiones o sin algunos enlaces. | Wiki incompleto o con poca claridad. | Wiki muy limitado o confuso. | No hay Wiki o está vacío. |
| **Pruebas de integración (H2)** | Implementa pruebas reales entre `Registry` y `RegistryRepository`. | ≥3 pruebas completas y funcionales, usando H2 y patrón AAA. | Pruebas funcionales pero con cobertura parcial. | Pruebas incompletas o sin verificación clara de persistencia. | Escenarios incorrectos o sin H2 configurado. | No existen pruebas de integración. |
| **Pruebas con mocks (Mockito)** | Uso de mocks y verificación de interacciones. | ≥2 pruebas con Mockito usando `when`, `verify`, `never`, etc. correctamente. | Pruebas correctas pero con poca variedad o validación parcial. | Usa mocks sin verificar interacciones o comportamiento. | Configuración incorrecta de mocks. | No existen pruebas con mocks. |
| **Pruebas de sistema (HTTP)** | Validación de endpoints reales con MockMvc o RestTemplate. | ≥2 pruebas HTTP completas (200, 400, 500), con aserciones válidas. | Pruebas funcionales pero con casos limitados. | Pruebas incompletas o con endpoints incorrectos. | Pruebas fallidas o sin conexión al servidor. | No existen pruebas HTTP. |
| **Cobertura de pruebas (JaCoCo)** | Nivel de cobertura global y por capa. | ≥80% global y ≥70% en `application` y `delivery`. | Entre 70–79% global, sin grandes omisiones. | Cobertura media (50–69%) o irregular. | Cobertura <50%. | No presenta reporte o no genera cobertura. |
| **Matriz de pruebas** | Tabla de casos probados y correspondencia con métodos de test. | Matriz completa, clara y actualizada. | Matriz parcial con algunos casos omitidos. | Matriz incompleta o sin correspondencia con código. | Matriz confusa o sin formato. | No entrega matriz. |
| **Gestión de defectos** | Registro de defectos y análisis. | Documento `defectos.md` con al menos 1 caso bien analizado. | Documento con casos simulados pero comprensibles. | Documento incompleto o superficial. | Caso sin análisis o sin evidencias. | No entrega `defectos.md`. |
| **Calidad del código** | Claridad, limpieza y consistencia del código. | Código limpio, sin duplicaciones, constantes extraídas, buen uso de excepciones. | Código comprensible con leves redundancias. | Código con duplicación o nombres poco claros. | Código confuso o sin buenas prácticas. | Código desorganizado o con errores graves. |
| **Reflexión técnica** | Análisis de resultados y aprendizajes. | Reflexión profunda sobre diseño, pruebas y CI/CD. | Reflexión correcta pero superficial. | Reflexión breve o poco argumentada. | Reflexión vaga o sin relación con el taller. | No presenta reflexión. |

| Rango de puntaje | Desempeño                                                |
| ---------------- | -------------------------------------------------------- |
| 45 – 50          | Excelente dominio técnico y metodológico.                |
| 35 – 44          | Buen trabajo con documentación o cobertura parcial.      |
| 30 – 34          | Cumple con lo básico pero sin profundidad.               |
| < 30             | No cumple con los criterios mínimos del taller/proyecto. |

---

