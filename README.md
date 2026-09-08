# 🎮 Análisis del mercado de videojuegos — Ice

## 📌 Descripción del proyecto

**Ice** es una tienda online de videojuegos que vende productos en diferentes mercados internacionales.

El objetivo de este proyecto fue analizar datos históricos de ventas, plataformas, géneros, calificaciones de usuarios y críticos, y clasificaciones ESRB para identificar **patrones asociados al éxito comercial de un videojuego**.

El análisis está planteado desde diciembre de 2016, con el objetivo de generar información útil para **planificar campañas publicitarias para 2017**.

Las principales preguntas del estudio fueron:

- ¿Qué plataformas y géneros han generado mayores ventas?
- ¿Cómo cambia el comportamiento del mercado entre Norteamérica, Europa y Japón?
- ¿Existe relación entre las reseñas y las ventas?
- ¿Qué características podrían ayudar a identificar videojuegos comercialmente prometedores?
- ¿Existen diferencias estadísticamente significativas entre las calificaciones de determinadas plataformas y géneros?

---

# 1. 🎯 Objetivo / problema de negocio

El objetivo fue identificar los factores que deberían considerarse al decidir **qué videojuegos, géneros y plataformas priorizar en futuras campañas de marketing**.

Para ello se analizaron:

- evolución histórica del número de lanzamientos;
- ciclo de vida de las plataformas;
- ventas por plataforma;
- ventas por género;
- diferencias regionales;
- clasificaciones ESRB;
- relación entre reseñas y ventas;
- comportamiento de juegos multiplataforma;
- pruebas estadísticas sobre calificaciones de usuarios.

---

# 2. 📂 Datos

El análisis se realizó utilizando:

`games.csv`

El dataset original contiene **16,715 registros** y 11 variables relacionadas con videojuegos publicados en diferentes años y regiones.

Después de eliminar dos registros sin nombre, el análisis quedó compuesto por **16,713 registros**.

## Variables principales

- `name`: nombre del videojuego.
- `platform`: plataforma.
- `year_of_release`: año de lanzamiento.
- `genre`: género.
- `na_sales`: ventas en Norteamérica, en millones.
- `eu_sales`: ventas en Europa, en millones.
- `jp_sales`: ventas en Japón, en millones.
- `other_sales`: ventas en otras regiones.
- `critic_score`: calificación de críticos, máximo 100.
- `user_score`: calificación de usuarios, máximo 10.
- `rating`: clasificación ESRB.

Además, se creó:

- `total_sales`: suma de ventas de todas las regiones.

---

# 3. 🛠️ Herramientas y tecnologías

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **SciPy**
- **Jupyter Notebook**

## Técnicas utilizadas

- **Data Cleaning**
- **Data Wrangling**
- **Exploratory Data Analysis (EDA)**
- Análisis de tendencias
- Análisis de ventas
- Segmentación por región
- Correlación
- Visualización de datos
- Pruebas de hipótesis
- Welch's t-test

---

# 4. 🔎 Proceso de análisis

## 4.1 Preparación de datos

Se revisaron:

- estructura del dataset;
- tipos de datos;
- registros duplicados;
- valores ausentes;
- consistencia de las variables.

Los nombres de las columnas fueron convertidos a minúsculas.

También se modificaron los tipos de datos de:

- `year_of_release`
- `critic_score`
- `user_score`

La variable `user_score` contenía el valor `"tbd"` (*to be determined*), por lo que fue convertida a formato numérico utilizando:

```python
pd.to_numeric(..., errors="coerce")
```

Los valores `"tbd"` quedaron representados como valores ausentes.

---

## 4.2 Tratamiento de valores ausentes

El dataset presentaba valores ausentes principalmente en:

- `year_of_release`
- `critic_score`
- `user_score`
- `rating`

Se eliminaron únicamente los dos registros sin nombre.

Los valores ausentes restantes se conservaron para evitar introducir información artificial mediante imputaciones que pudieran alterar el análisis.

Después del procesamiento:

- `year_of_release`: **269 valores ausentes**
- `critic_score`: **8,576**
- `user_score`: **9,123**
- `rating`: **6,764**

---

## 4.3 Cálculo de ventas globales

Se creó la variable:

`total_sales`

calculada como:

```python
games["total_sales"] = (
    games["na_sales"]
    + games["eu_sales"]
    + games["jp_sales"]
    + games["other_sales"]
)
```

Las ventas globales acumuladas del dataset fueron aproximadamente:

**8,913.29 millones**

---

# 5. 📈 Evolución histórica del mercado

Se analizó el número de videojuegos publicados por año.

Los lanzamientos aumentaron considerablemente durante la década de los 2000 y alcanzaron sus niveles más altos alrededor de:

- **2008: 1,427 juegos**
- **2009: 1,426 juegos**
- **2010: 1,255 juegos**
- **2011: 1,136 juegos**

Después de 2011 se observa una reducción importante en el número de títulos registrados.

---

# 6. 🎮 Rendimiento por plataforma

Las plataformas con mayores ventas históricas fueron:

| Plataforma | Ventas globales |
|---|---:|
| PS2 | **1,255.77** |
| X360 | **971.42** |
| PS3 | **939.65** |
| Wii | **907.51** |
| DS | **806.12** |
| PS | **730.86** |

El análisis histórico muestra que las plataformas tienen ciclos de vida limitados.

Entre las plataformas principales, muchas permanecieron activas aproximadamente durante **7 a 11 años**, aunque existen excepciones como PC.

## Plataformas que perdieron relevancia

Al comparar periodos históricos se observaron plataformas que tuvieron ventas elevadas y posteriormente prácticamente desaparecieron del mercado, entre ellas:

- PlayStation original
- Game Boy Advance
- Game Boy
- Xbox
- Nintendo 64
- NES
- SNES
- GameCube

Esto refuerza la importancia de considerar **el momento del ciclo de vida de una plataforma**, no solamente sus ventas históricas acumuladas.

---

# 7. 📊 Distribución de ventas por plataforma

Las ventas históricas presentan una distribución fuertemente sesgada.

Entre las plataformas:

- venta media acumulada: aproximadamente **287.5**
- mediana: aproximadamente **200.0**
- máximo: **1,255.77**

La diferencia entre media y mediana indica que un grupo reducido de plataformas concentra una proporción elevada de las ventas totales.

Por este motivo, las ventas históricas deben interpretarse junto con:

- vigencia de la plataforma;
- año;
- tendencia reciente;
- tamaño de catálogo.

---

# 8. ⭐ Relación entre calificaciones y ventas

Se analizaron las relaciones entre:

- `critic_score`
- `user_score`
- `total_sales`

En el dataset completo, las correlaciones aproximadas fueron:

| Variable | Correlación con ventas |
|---|---:|
| `critic_score` | **0.25** |
| `user_score` | **0.09** |

Esto indica que ninguna de las dos variables presenta por sí sola una relación fuerte con las ventas.

Sin embargo, las reseñas de críticos presentan una asociación algo mayor que las calificaciones de usuarios.

## Xbox 360

Al analizar específicamente Xbox 360:

- correlación entre `critic_score` y ventas: **≈ 0.39**
- correlación entre `user_score` y ventas: **≈ 0.11**

Por tanto, en esta plataforma las reseñas de críticos presentan una relación positiva moderada con las ventas, mientras que las calificaciones de usuarios muestran una relación mucho más débil.

Las puntuaciones deben interpretarse como **un factor adicional**, no como un predictor único del éxito comercial.

---

# 9. 🌍 Videojuegos multiplataforma

También se analizaron títulos publicados en varias plataformas.

Los videojuegos multiplataforma con mayores ventas acumuladas incluyeron:

| Juego | Plataformas | Ventas totales |
|---|---:|---:|
| Grand Theft Auto V | 5 | **56.58** |
| Call of Duty: Black Ops | 5 | **30.82** |
| Call of Duty: Modern Warfare 3 | 4 | **30.60** |
| Call of Duty: Black Ops II | 4 | **29.40** |
| Call of Duty: Ghosts | 6 | **27.39** |

## Grand Theft Auto V

Las ventas variaron significativamente dependiendo de la plataforma:

| Plataforma | Ventas |
|---|---:|
| PS3 | **21.05** |
| X360 | **16.27** |
| PS4 | **12.62** |
| XOne | **5.47** |
| PC | **1.17** |

Esto demuestra que **el mismo videojuego puede presentar resultados comerciales muy diferentes según la plataforma**.

---

# 10. 🕹️ Análisis por género

Los géneros con mayores ventas históricas fueron:

| Género | Ventas globales |
|---|---:|
| Action | **1,744.17** |
| Sports | **1,331.27** |
| Shooter | **1,052.45** |
| Role-Playing | **934.56** |
| Platform | **827.77** |

Los géneros con menores ventas acumuladas fueron:

- Strategy
- Adventure
- Puzzle

El género es un factor importante para la planificación comercial, pero su desempeño cambia notablemente según el mercado regional.

---

# 11. 🌎 Perfil de usuario por región

## Norteamérica

Principales plataformas históricas:

1. **X360 — 602.47**
2. **PS2 — 583.84**
3. **Wii — 496.90**
4. **PS3 — 393.49**
5. **DS — 382.40**

Los géneros con mayores ventas fueron:

- Action
- Sports
- Shooter
- Platform
- Misc

## Europa

Principales plataformas históricas:

1. **PS2 — 339.29**
2. **PS3 — 330.29**
3. **X360 — 270.76**
4. **Wii — 262.21**
5. **PS — 213.61**

Los géneros principales fueron:

- Action
- Sports
- Shooter
- Racing
- Misc

## Japón

Principales plataformas históricas:

1. **DS — 175.57**
2. **PS — 139.82**
3. **PS2 — 139.20**
4. **SNES — 116.55**
5. **3DS — 100.67**

El comportamiento japonés fue considerablemente diferente.

Los géneros principales fueron:

- Role-Playing
- Action
- Sports
- Platform
- Misc

Destaca especialmente **Role-Playing**, que lidera ampliamente el mercado japonés.

Por otro lado, `Shooter`, uno de los géneros principales en Norteamérica y Europa, presenta una participación mucho menor en Japón.

---

# 12. 🔞 Clasificación ESRB

Las categorías ESRB con mayores ventas globales fueron:

| Clasificación | Ventas globales |
|---|---:|
| E | **2,435.52** |
| T | **1,493.35** |
| M | **1,473.79** |
| E10+ | **655.60** |

La clasificación **E — Everyone** presentó el mayor volumen histórico.

Sin embargo, la clasificación ESRB no debe interpretarse de forma aislada, ya que también está relacionada con factores como:

- género;
- audiencia objetivo;
- región;
- cantidad de juegos disponibles.

---

# 13. 🧪 Pruebas de hipótesis

Se utilizó un nivel de significancia:

**α = 0.05**

y una prueba **t de Welch para muestras independientes**, debido a que no era necesario asumir varianzas iguales entre los grupos.

## Hipótesis 1 — Xbox One vs PC

### Hipótesis nula

**H₀:** las calificaciones promedio de usuarios de Xbox One y PC son iguales.

### Hipótesis alternativa

**H₁:** las calificaciones promedio son diferentes.

Resultado:

**p-value ≈ 0.00000494**

Como:

`p-value < 0.05`

se rechazó la hipótesis nula.

### Conclusión

Existe evidencia estadística suficiente para considerar que las calificaciones promedio de usuarios de **Xbox One y PC son diferentes**.

Las medias observadas fueron aproximadamente:

- Xbox One: **6.52**
- PC: **7.06**

## Hipótesis 2 — Acción vs Deportes

### Hipótesis nula

**H₀:** las calificaciones promedio de usuarios de Action y Sports son iguales.

### Hipótesis alternativa

**H₁:** las calificaciones promedio son diferentes.

Resultado:

**p-value ≈ 0.115**

Como:

`p-value > 0.05`

no existe evidencia suficiente para rechazar la hipótesis nula.

### Conclusión

No se encontró una diferencia estadísticamente significativa entre las calificaciones promedio de los géneros **Action y Sports**.

Las medias fueron aproximadamente:

- Action: **7.05**
- Sports: **6.96**

---

# 14. 📊 Hallazgos principales

### 🔹 1. El éxito comercial depende fuertemente de la plataforma

El mismo videojuego puede generar resultados muy distintos dependiendo de dónde se publique.

Grand Theft Auto V, por ejemplo, registró ventas desde **21.05 en PS3** hasta **1.17 en PC**.

### 🔹 2. Las preferencias cambian considerablemente por región

Norteamérica y Europa presentan un fuerte peso de:

- Action
- Sports
- Shooter

Mientras que Japón destaca especialmente por:

- Role-Playing

Esto significa que una campaña global debería adaptarse al mercado objetivo.

### 🔹 3. Las calificaciones no explican por sí solas el éxito

La relación entre ventas y `user_score` fue muy débil.

Las reseñas de críticos mostraron una relación algo mayor, especialmente en Xbox 360, pero tampoco son suficientes para determinar por sí solas si un videojuego tendrá éxito.

### 🔹 4. El ciclo de vida de las plataformas es fundamental

Una plataforma puede haber generado ventas históricas muy elevadas y ya no ser una opción atractiva para una campaña futura.

Por ello, las decisiones de marketing deben priorizar:

> **Ventas recientes + Tendencia + Plataforma + Región + Género**

y no únicamente ventas acumuladas.

---

# 15. 💡 Conclusiones y recomendaciones

Los resultados muestran que el éxito de un videojuego no depende de una única variable.

Para planificar campañas publicitarias, Ice debería analizar conjuntamente:

> **Plataforma + Región + Género + Tendencia de ventas + Ciclo de vida + Audiencia**

## Recomendaciones

### ✅ 1. Adaptar campañas por región

Las preferencias de usuarios cambian notablemente entre mercados.

Por ejemplo:

- Norteamérica y Europa presentan mayor demanda de Action, Sports y Shooter.
- Japón presenta una fuerte preferencia por Role-Playing.

Una estrategia publicitaria única para todas las regiones probablemente sería menos eficiente.

### ✅ 2. Priorizar plataformas vigentes

Las ventas históricas pueden ser engañosas.

Una plataforma como PS2 tuvo las mayores ventas acumuladas del dataset, pero ya había terminado prácticamente su ciclo comercial.

Para campañas futuras deben priorizarse plataformas con:

- actividad reciente;
- base de usuarios vigente;
- tendencia favorable.

### ✅ 3. Utilizar las reseñas como señal complementaria

Las puntuaciones de críticos y usuarios pueden aportar información sobre recepción y calidad percibida, pero no deberían utilizarse como criterio principal para estimar ventas.

Variables como plataforma, género, región y momento del mercado tienen un peso comercial más evidente.

### ✅ 4. Segmentar la inversión publicitaria

Ice debería asignar presupuesto según combinaciones específicas:

> **Región + Plataforma + Género**

en lugar de promocionar todos los juegos de la misma forma.

---

# 16. 📈 Visualizaciones principales

Las visualizaciones más importantes para comunicar el proyecto son:

## 1. Lanzamientos de videojuegos por año

Permite observar cómo evolucionó la industria y detectar periodos de expansión y contracción.
<img width="1014" height="569" alt="image" src="https://github.com/user-attachments/assets/92a0a019-ba76-400e-89f6-d5dea3891a84" />


## 2. Ventas globales por plataforma

Permite comparar el tamaño comercial histórico de las diferentes plataformas y visualizar la concentración de ventas.
<img width="580" height="417" alt="image" src="https://github.com/user-attachments/assets/1079939a-3b98-45c3-aa50-f26194bbbd96" />

---

# 17. 📁 Estructura del proyecto

```text
Project-SP6/
│
├── Proyecto_ipynb_SP6.ipynb
├── games.csv
└── README.md
```
