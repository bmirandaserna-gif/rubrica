---
name: generar-rubrica
description: Genera un borrador de rúbrica de evaluación en formato Markdown a partir de una actividad evaluable descrita en un documento de texto (TXT, MD) o PDF. Úsala cuando se pida generar una rúbrica, proponer criterios de evaluación, definir niveles y valores de una rúbrica, o cuando se invoque el comando /rubrica.
---

# Generar rúbrica de evaluación

Convierte una actividad evaluable descrita en un documento en una rúbrica de
evaluación en formato Markdown.

## Responsabilidades y límites

Quién la invoca: comando `/rubrica` a través del agente `rubrica`. Esta skill
aporta la metodología y el conocimiento experto; no realiza la lectura del
documento (lo hace el agente antes de activar el flujo).

## Flujo de trabajo

### Paso 1. Analizar la actividad

- Leer el contenido del documento proporcionado e identificar los **objetivos
  de aprendizaje** que evalúa (operativos, conceptuales, procedimentales).
- Identificar las **tareas concretas** exigidas (cálculos, conversiones,
  definiciones, ejercicios) y la estructura (número de apartados).
- Detectar el **tipo de actividad**: en tareas de conversión numérica deben
  evaluarse tanto el _resultado_ como el _procedimiento_ y las _definiciones_.

### Paso 2. Solicitar el número de criterios y niveles

- Preguntar siempre al usuario el **número de criterios** y el **número de
  niveles** de la rúbrica antes de proponerla.
- Si el usuario no los especifica, usar los valores por defecto:
  - **5 criterios** de evaluación.
  - **4 niveles** de logro.

### Paso 3. Proponer los criterios de evaluación

Heurística para actividades de conversión binario-decimal como la Tarea 1:

- **Aplicación del procedimiento/algoritmo** (por ejemplo, el teorema
  fundamental de la numeración): uso correcto de la base, de las potencias y de
  la suma de los productos posicionales.
- **Conversión en un sentido** (p. ej. binario → decimal): todos los casos
  resueltos, con resultados correctos y desempeño ante casos con decimales o
  comas.
- **Conversión en el sentido contrario** (p. ej. decimal → binario): divisiones
  sucesivas o de la parte decimal según proceda, resultados correctos.
- **Conceptos teóricos**: definiciones correctas de los conceptos implicados
  (informática, código binario) y explicación del proceso de entrada,
  procesamiento y salida de datos (entrada → proceso → pantalla).
- **Presentación y entrega**: claridad, orden, justificación de los cálculos,
  realización de las tareas complementarias (ejercicio del libro), ortografía.

Reglas:
- Cada criterio debe ser **observable y medible**.
- Los criterios deben cubrir la totalidad de los apartados del documento sin
  dejarse ninguno fuera (si el documento menciona un ejercicio de libro o unas
  preguntas conceptuales, deben verse reflejados).
- Adaptar el nombre del criterio al vocabulario de la actividad.

### Paso 4. Proponer descripciones y valores numéricos por nivel

Niveles por defecto (4):

| Nivel | Etiqueta | Valor |
|---|---|---|
| 4 | Excelente | 10 |
| 3 | Bueno | 7 |
| 2 | Suficiente | 5 |
| 1 | Insuficiente | 0 |

Con **N niveles**, se usan valores proporcionales repartidos entre la nota
máxima (10) y la mínima (0), con etiquetas equivalentes.

Reglas para las descripciones:
- Cada nivel se describe con una frase **cualitativa y específica** del
  criterio, no genérica.
- Los niveles deben ser **progresivos** (cada nivel supone más logro que el
  anterior) y **exhaustivos** (describen todo el continuo de posibles logros).
- Incluir en la descripción las condiciones "todo/some/nada" cuando sea
  pertinente (p. ej. "resuelve **todos** los casos correctamente").

Validar los valores propuestos: indicar si las conversiones de ejemplo son
correctas o erróneas de forma explícita y coherente entre niveles.

### Paso 5. Devolver la rúbrica en Markdown

- Emitir una **tabla Markdown** con una columna por criterio y una columna por
  nivel, incluyendo etiqueta y valor numérico en la cabecera.
- Decorar con los **códigos decimales/correspondencia correctos** solo cuando
  aporte valor pedagógico (p. ej. resultados de referencia en las descripciones
  de los niveles altos).
- Si se pide, guardar la rúbrica en un archivo `.md`.

### Plantilla de salida

```markdown
| Criterio | Excelente (10) | Bueno (7) | Suficiente (5) | Insuficiente (0) |
|---|---|---|---|---|
| <Criterio 1> | ... | ... | ... | ... |
| <Criterio 2> | ... | ... | ... | ... |
...
```

## Lectura del documento

- Si el documento es un PDF y la herramienta `Read` no puede extraer el texto,
  extraerlo con el método disponible en el entorno (p. ej. Python con una
  librería local, `pdftotext`, o la extracción manual con `zlib` sobre el
  flujo `stream` del PDF). El resultado esperado es el texto plano de la
  actividad, del que se extraen las tareas a evaluar.