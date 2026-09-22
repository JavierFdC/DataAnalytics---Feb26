# Proyecto Final - Data analytics
## MetroBus Analytics — De los datos crudos al cuadro de mando

## 1. Presentación del proyecto

Este proyecto es el cierre del curso.Una demostración de que sabes hacer el trabajo de un analista de datos de principio a fin.

Durante el curso has aprendido a manejar datos con SQL, a transformarlos y analizarlos con Python, a construir pipelines ETL, a aplicar principios de gobierno del dato y a visualizar resultados con herramientas de BI. El proyecto integra todas esas piezas sobre un único caso de negocio real: **MetroBus**, una operadora ficticia de autobuses urbanos.

El resultado esperado no es solo código que funciona o un dashboard que se ve bien. Es un proyecto coherente, documentado y defendible: el tipo de entrega que presentarías en una empresa.

---

## 2. El caso de negocio: MetroBus

**MetroBus** es una operadora pública de autobuses urbanos. Opera 10 líneas (urbanas, interurbanas y nocturnas), una flota de 45 vehículos con distintos tipos de combustible (diésel, eléctrico e híbrido) y 28 conductores activos distribuidos en tres cocheras.

Históricamente, la toma de decisiones se ha basado en informes manuales y en la experiencia de los responsables de flota. La dirección quiere transformar la operación con datos: detectar ineficiencias, anticipar averías, entender la demanda por línea y franja horaria, y justificar inversiones en flota eléctrica con evidencia cuantitativa.

Tu rol es el de analista de datos responsable de construir la infraestructura analítica completa: desde la ingesta del dato crudo hasta el cuadro de mando que verá la dirección.

---

## 3. El dataset

Recibes **9 archivos CSV** que representan los datos históricos de MetroBus entre 2022 y 2024:

### Tablas de hechos

| Archivo | Descripción | Filas |
|---|---|---|
| `fact_viajes.csv` | Una fila por expedición: tiempos programados y reales, ocupación, consumo, franja horaria y tarifa predominante. | ~50.000 |
| `fact_incidencias.csv` | Incidencias registradas durante viajes: averías, accidentes, retenciones, vandalismo. Incluye severidad y coste estimado. | ~4.000 |
| `fact_mantenimiento.csv` | Historial de intervenciones por vehículo: preventivas, correctivas, legales y estéticas. | ~900 |

### Tablas de dimensión

| Archivo | Descripción |
|---|---|
| `dim_linea.csv` | Tipo de línea, km de recorrido, número de paradas, frecuencia programada. |
| `dim_vehiculo.csv` | Modelo, combustible, capacidad, año de fabricación, emisiones CO₂. |
| `dim_conductor.csv` | Antigüedad, turno habitual, cochera, formación y tipo de licencia. |
| `dim_parada.csv` | Barrio, coordenadas GPS, tipo de parada, accesibilidad. |
| `dim_tarifa.csv` | Tipos de título: ordinario, bono, abono mensual, turístico, escolar, gratuito social. |
| `dim_depot.csv` | Las tres cocheras: ubicación y capacidad. |

> **Advertencia:** el dataset contiene errores intencionales que reflejan la realidad de los datos en producción. Detectarlos, documentarlos y resolverlos es parte del proyecto.

---

## 4. Fases del proyecto

El proyecto está organizado en cinco fases que siguen el flujo de trabajo real de un analista de datos. Cada fase se corresponde con un bloque del curso.

---

### Fase 1 — Exploración y análisis con Python

*Bloque: Python / Pandas / EDA*

Antes de construir nada, necesitas entender qué tienes. Usa Python y pandas para hacer un análisis exploratorio del dataset.

**Entregable:** un notebook Jupyter (`01_eda.ipynb`) con:

- Carga de los 9 CSV y revisión de dimensiones, tipos y primeras filas de cada tabla.
- Análisis de valores nulos por tabla y columna: ¿qué porcentaje representa cada uno? ¿Hay un patrón?
- Análisis de outliers. Usa distribuciones o estadísticos descriptivos para argumentar qué es un outlier y qué no.
- Al menos **3 visualizaciones exploratorias** con matplotlib o seaborn que respondan a preguntas de negocio reales.
- Un apartado de conclusiones (en markdown dentro del notebook) con los hallazgos más relevantes: qué te sorprende, qué problemas detectas, qué hipótesis te genera el dataset.

---

### Fase 2 — Modelo relacional y SQL

*Bloque: SQL y bases de datos*

El análisis exploratorio te ha dado contexto. Ahora vas a estructurar los datos en un modelo relacional y a responder preguntas de negocio con SQL.

**Entregable:** un archivo `02_sql.sql` con:

- DDL completo para crear las 9 tablas con claves primarias, claves foráneas y tipos de dato correctos.
- Carga de los CSV en la base de datos (puede ser con Python + el conector correspondiente, documentado en el README).
- **Al menos 8 consultas SQL** que respondan a preguntas de negocio que tú consideres relevantes.

Cada consulta debe ir precedida de un comentario que explique en una línea qué pregunta de negocio responde.

---

### Fase 3 — Gobierno del dato

*Bloque: Cloud y gobierno del dato*

Un pipeline que nadie entiende y que nadie puede auditar no sirve para producción. Esta fase documenta las decisiones de calidad y define las reglas de negocio del dataset.

**Entregable:** un archivo `03_gobierno.md` con:

**Diccionario de datos:**
Una tabla para cada una de las 9 tablas del modelo (las 3 de hechos y las 6 de dimensión) con las columnas: nombre de campo, tipo de dato, descripción, valores válidos o rango esperado, y observaciones de calidad detectadas.

**Registro de decisiones de limpieza (Data Quality Log):**
Una tabla con al menos **8 entradas**, una por cada problema de calidad identificado y resuelto. Columnas: tabla, campo, tipo de problema, frecuencia (nº de filas afectadas), decisión tomada, justificación.

**Definición formal de KPIs:**
Define con precisión las métricas que usará el dashboard. Para cada KPI indica: nombre, fórmula exacta, fuente de datos (tabla y campo), criterios de exclusión si los hay, y el responsable de negocio que debería validarla. Mínimo 6 KPIs.

---

### Fase 4 — Cuadro de mando

*Bloque: Visualización y storytelling (Power BI, Tableau o Qlik)*

Con los datos limpios y el modelo definido, construyes el cuadro de mando que verá la dirección de MetroBus. Lo que se evalúa es el resultado, no la herramienta.

**Herramienta:** Power BI Desktop, Tableau Public o Qlik Sense. La elección es tuya. Lo que se evalúa es el resultado, no la herramienta.

**Requisitos del modelo de datos:**
- Esquema en estrella con las 3 tablas de hechos y sus dimensiones correctamente relacionadas.
- Tabla de calendario creada y marcada como dimensión temporal oficial, con columnas de año, mes, trimestre, semana y franja horaria.
- Todas las relaciones con cardinalidad muchos a uno y dirección de filtro correcta.

**Principios de diseño que se evaluarán:**
- Regla de los 3 segundos: la pregunta principal de cada página se responde en 3 segundos.
- Número máximo de elementos por página.
- Organización de la estructura de las páginas.
- Selección correcta del tipo de gráfico para cada tipo de pregunta.
- Segmentadores sincronizados entre páginas.
- Navegación entre las páginas.
---

### Fase 5 — Presentación y storytelling

*Bloque: Visualización y storytelling*

Un analista que no sabe comunicar sus hallazgos es la mitad de un analista. Esta fase es la narración del proyecto: no el manual de usuario del dashboard, sino la historia de lo que descubriste y qué significa para MetroBus.

**Entregable:** una presentación de **máximo 8 diapositivas** (`05_presentacion.pdf` o PowerPoint) con la siguiente estructura:

1. **El problema**: qué situación tenía MetroBus antes del proyecto.
2. **Los datos**: qué dataset usaste, qué período cubre, qué calidad tenían los datos.
3. **Los hallazgos**: los 3 insights más relevantes que encontraste. Cada insight va acompañado del gráfico que lo soporta y una frase de conclusión accionable.
4. **Las recomendaciones**: qué le dirías a la dirección de MetroBus que debería hacer diferente.
5. **Las limitaciones**: qué no puedes decir con estos datos, qué dato adicional necesitarías.
6. **Próximos pasos**: qué añadirías al proyecto si tuvieras más tiempo.

La presentación no es un manual de usuario del dashboard. Es la historia de lo que descubriste en los datos y qué significa para MetroBus.

---

## 5. Estructura de entrega

```
Apellido_Nombre_ProyectoFinal/
│
├── README.md                        ← Obligatorio. Ver sección 6.
│
├── 01_eda.ipynb                     ← Fase 1: Exploración con Python
├── 02_sql.sql                       ← Fase 2: Modelo relacional y consultas
├── 03_gobierno.md                   ← Fase 3: Gobierno del dato
├── 04_dashboard.*                   ← Fase 4: Archivo de la herramienta BI
│                                       (.pbix / .twbx / .qvf)
├── 05_presentacion.pdf              ← Fase 5: Presentación narrativa
│
└── data/
    ├── fact_viajes.csv
    ├── fact_incidencias.csv
    ├── fact_mantenimiento.csv
    ├── dim_linea.csv
    ├── dim_vehiculo.csv
    ├── dim_conductor.csv
    ├── dim_parada.csv
    ├── dim_tarifa.csv
    └── dim_depot.csv
```

Formato de entrega: carpeta comprimida `Apellido_Nombre_ProyectoFinal.zip`.

---

## 6. El README

El `README.md` es un documento técnico de **máximo 500 palabras** que debe cubrir:

1. **Stack tecnológico usado:** versión de Python, librerías principales, herramienta BI elegida, base de datos para SQL.
2. **Cómo ejecutar el proyecto:** instrucciones para que alguien pueda reproducir las fases 1 y 2 desde cero en su máquina.
3. **Decisiones no obvias:** una decisión técnica o de negocio que tomaste y que no está en el enunciado. Por qué la tomaste.
4. **El hallazgo más importante:** en dos frases, qué es lo más relevante que encontraste en los datos de MetroBus.

---

## 7. Criterios de evaluación

| Fase | Peso | Descripción |
|---|---|---|
| **Fase 1 — EDA con Python** | 20% | Rigor del análisis, calidad de visualizaciones, claridad de conclusiones. |
| **Fase 2 — SQL** | 20% | DDL correcto, consultas que responden preguntas reales, uso de JOINs, agrupaciones o  CTEs. |
| **Fase 3 — Gobierno** | 15% | Diccionario completo, Data Quality Log con 8+ entradas, KPIs definidos con precisión. |
| **Fase 4 — Dashboard** | 30% | Modelo en estrella correcto, KPIs exactos, páginas funcionales, principios de diseño aplicados. |
| **Fase 5 — Presentación** | 10% | Estructura narrativa, calidad de los insights, recomendaciones accionables. |
| **README** | 5% | Reproducibilidad, claridad, decisión no obvia documentada. |

---

## 8. Normas

- El proyecto es **individual**. Está permitido consultar documentación, recursos online y los materiales del curso. No está permitido compartir código, notebooks ni archivos de proyecto con otros alumnos.
- El uso de herramientas de IA (ChatGPT, Copilot, etc.) está permitido como apoyo, no como sustituto del trabajo. Si una parte del código fue generada con IA, indícalo en el README con una línea. Lo que se evalúa es que entiendes lo que entregas.
- En caso de duda técnica, usa los canales habilitados. Una duda individual de un alumno puede ser una pregunta útil para el resto de compañeros.

---

*Proyecto Final MetroBus · Curso de Data Analytics · Promociones 2025 y 2026*
