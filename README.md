# 🇨🇴 Mortalidad en Colombia 2019 — Dashboard Interactivo

## Introducción

Esta aplicación web interactiva permite analizar y explorar la mortalidad en Colombia durante el año 2019, utilizando datos provenientes de las **Estadísticas Vitales (Defunciones No Fetales)** del **Departamento Administrativo Nacional de Estadística (DANE)**.

El dashboard integra **7 visualizaciones interactivas** construidas con **Plotly y Dash** en Python, que facilitan la interpretación de patrones demográficos, regionales y temporales de mortalidad. La herramienta permite a investigadores, estudiantes y tomadores de decisiones en salud pública explorar los datos de manera intuitiva y accesible.

**Proyecto académico** — Maestría en Inteligencia Artificial, Universidad de La Salle.

---

## Objetivo

Analizar la mortalidad en Colombia para el año 2019 a través de una aplicación web dinámica que permita:

- Identificar la distribución geográfica de las defunciones por departamento.
- Visualizar la variación temporal (mensual) de la mortalidad.
- Determinar las ciudades con mayor incidencia de homicidios por armas de fuego (código X95).
- Conocer las ciudades con menor índice de mortalidad.
- Listar las principales causas de muerte según la clasificación CIE-10.
- Comparar la mortalidad por sexo en cada departamento.
- Analizar la distribución de muertes por grupo de edad según las categorías DANE.

---

## Estructura del Proyecto

```
colombia-mortalidad-2019/
│
├── app.py                  # Aplicación principal (Dash + Plotly)
├── requirements.txt        # Dependencias de Python
├── Procfile                # Comando de inicio para Render/Heroku
├── render.yaml             # Configuración de despliegue en Render
├── runtime.txt             # Versión de Python para el despliegue
├── .gitignore              # Archivos excluidos de Git
├── README.md               # Este archivo
│
└── data/
    ├── .gitkeep            # Mantiene la carpeta en Git
    └── nofetal2019.csv     # (Opcional) Microdatos DANE descargados
```

### Descripción de archivos clave

| Archivo | Descripción |
|---------|-------------|
| `app.py` | Código completo de la aplicación: carga de datos, procesamiento, creación de figuras y layout del dashboard Dash. |
| `requirements.txt` | Lista de librerías necesarias con versiones específicas. |
| `Procfile` | Indica a Render/Heroku cómo arrancar la aplicación con Gunicorn. |
| `render.yaml` | Configuración declarativa para despliegue automático en Render. |
| `data/nofetal2019.csv` | Archivo CSV con los microdatos del DANE (se descarga manualmente). Si no se proporciona, la app genera datos sintéticos basados en las cifras oficiales. |

---

## Requisitos

### Librerías y versiones

| Librería | Versión | Propósito |
|----------|---------|-----------|
| `dash` | 2.18.2 | Framework web para dashboards interactivos |
| `plotly` | 5.24.1 | Motor de visualización de gráficos interactivos |
| `pandas` | 2.2.3 | Manipulación y análisis de datos tabulares |
| `numpy` | 1.26.4 | Operaciones numéricas y generación de datos sintéticos |
| `gunicorn` | 23.0.0 | Servidor WSGI para despliegue en producción |

### Entorno

- **Python**: 3.9 o superior (recomendado 3.11)
- **Sistema operativo**: Windows, macOS o Linux
- **Navegador**: Chrome, Firefox, Edge o Safari (actualizado)

---

## Software y Herramientas Utilizadas

| Herramienta | Uso |
|-------------|-----|
| **Python 3.11** | Lenguaje de programación principal |
| **Dash** | Framework web para la construcción del dashboard |
| **Plotly** | Librería de gráficos interactivos (mapas, líneas, barras, pie, histogramas) |
| **Pandas** | Procesamiento y transformación de los microdatos DANE |
| **NumPy** | Generación de distribuciones estadísticas para datos sintéticos |
| **Gunicorn** | Servidor HTTP WSGI para producción |
| **Render** | Plataforma PaaS para el despliegue en la nube |
| **Git + GitHub** | Control de versiones y repositorio remoto |
| **DANE - Estadísticas Vitales** | Fuente oficial de datos de mortalidad |

---

## Instalación Local

### Paso 1: Clonar el repositorio

```bash
git clone https://github.com/TU-USUARIO/colombia-mortalidad-2019.git
cd colombia-mortalidad-2019
```

### Paso 2: Crear un entorno virtual (recomendado)

```bash
python -m venv venv

# En Windows:
venv\Scripts\activate

# En macOS/Linux:
source venv/bin/activate
```

### Paso 3: Instalar las dependencias

```bash
pip install -r requirements.txt
```

### Paso 4: (Opcional) Agregar datos reales del DANE

Para usar los microdatos oficiales en lugar de datos sintéticos:

1. Ir a https://microdatos.dane.gov.co/index.php/catalog/696/get-microdata
2. Registrarse (gratis) y descargar el archivo de **defunciones no fetales 2019**.
3. Colocar el archivo `nofetal2019.csv` dentro de la carpeta `data/`.

> **Nota:** Si no se proporciona el CSV, la aplicación genera automáticamente ~242,609 registros sintéticos que replican la distribución real de mortalidad reportada por el DANE para 2019.

### Paso 5: Ejecutar la aplicación

```bash
python app.py
```

Abrir el navegador en: **http://localhost:8050**

---

## Despliegue en Render

### Paso 1: Subir el proyecto a GitHub

```bash
# Inicializar repositorio (si aún no existe)
git init
git add .
git commit -m "Dashboard mortalidad Colombia 2019"

# Crear repositorio en GitHub y conectarlo
git remote add origin https://github.com/TU-USUARIO/colombia-mortalidad-2019.git
git branch -M main
git push -u origin main
```

### Paso 2: Crear el servicio en Render

1. Ir a [https://render.com](https://render.com) e iniciar sesión (se puede usar la cuenta de GitHub).
2. Hacer clic en **"New +"** → **"Web Service"**.
3. Seleccionar **"Build and deploy from a Git repository"** → **Next**.
4. Conectar el repositorio de GitHub (`colombia-mortalidad-2019`).
5. Configurar los siguientes campos:

| Campo | Valor |
|-------|-------|
| **Name** | `colombia-mortalidad-2019` |
| **Region** | Oregon (US West) o el más cercano |
| **Branch** | `main` |
| **Runtime** | `Python 3` |
| **Build Command** | `pip install -r requirements.txt` |
| **Start Command** | `gunicorn app:server --bind 0.0.0.0:$PORT` |

6. Seleccionar el plan **Free** (gratuito).
7. Hacer clic en **"Create Web Service"**.

### Paso 3: Esperar el despliegue

Render construirá y desplegará la aplicación automáticamente. En unos 2-5 minutos estará disponible en una URL como:

```
https://colombia-mortalidad-2019.onrender.com
```

> **Nota:** El plan gratuito de Render pone la aplicación en modo "sleep" tras 15 minutos de inactividad. La primera carga después puede tardar 30-60 segundos.

---

## Visualizaciones y Explicación de Resultados

### 1. 🗺️ Mapa — Distribución de Muertes por Departamento

**Tipo:** Mapa de burbujas sobre Mapbox (mapa oscuro).

**Descripción:** Muestra la distribución geográfica del total de defunciones en cada uno de los 33 departamentos de Colombia. El tamaño y color de cada burbuja representa el volumen de muertes.

**Hallazgos clave:**
- **Bogotá D.C., Antioquia y Valle del Cauca** concentran la mayor cantidad de defunciones, lo cual es consistente con su alta densidad poblacional.
- Los departamentos de la Amazonía y Orinoquía (Vaupés, Guainía, Vichada) registran los menores totales, correlacionados con poblaciones pequeñas y dispersas.
- Cundinamarca aparece como un departamento con alta mortalidad debido a la influencia del área metropolitana de Bogotá.

---

### 2. 📈 Gráfico de Líneas — Total de Muertes por Mes

**Tipo:** Gráfico de línea con relleno y marcadores.

**Descripción:** Representa la variación mensual del total de defunciones a lo largo de 2019, permitiendo identificar picos estacionales.

**Hallazgos clave:**
- Los meses de **enero y diciembre** presentan los picos más altos de mortalidad, fenómeno asociado a factores respiratorios estacionales, festividades (accidentalidad) y temperaturas bajas en zonas andinas.
- **Junio y septiembre** muestran los valores más bajos del año.
- La curva muestra un patrón de "U": alta al inicio, descenso hacia mediados de año y ascenso al final.

---

### 3. 📊 Gráfico de Barras — Top 5 Ciudades Más Violentas

**Tipo:** Gráfico de barras horizontal con escala de color rojo.

**Descripción:** Muestra las 5 ciudades con mayor número de homicidios codificados como **X95** (agresión con disparo de armas de fuego, incluyendo los no especificados) según la clasificación CIE-10.

**Hallazgos clave:**
- **Cali** lidera consistentemente las cifras de homicidios por armas de fuego, reflejando la problemática de violencia urbana en el suroccidente colombiano.
- **Bogotá y Medellín** aparecen en las siguientes posiciones, aunque con tasas por 100,000 hab. potencialmente menores dada su mayor población.
- Ciudades intermedias como **Cúcuta** reflejan la influencia de la situación fronteriza.

---

### 4. 🥧 Gráfico Circular — 10 Ciudades con Menor Mortalidad

**Tipo:** Gráfico de dona (pie chart con agujero central).

**Descripción:** Presenta las 10 ciudades identificadas que registraron el menor número de defunciones totales.

**Hallazgos clave:**
- Las ciudades con menor mortalidad tienden a ser municipios pequeños o ciudades intermedias con poblaciones reducidas.
- La distribución es relativamente homogénea entre las 10 ciudades, sin una que domine significativamente.

---

### 5. 📋 Tabla — Top 10 Causas de Muerte

**Tipo:** Tabla interactiva con código CIE-10, nombre y total de casos.

**Descripción:** Lista las 10 principales causas de muerte en Colombia para 2019, ordenadas de mayor a menor número de casos.

**Hallazgos clave:**
- Las **enfermedades isquémicas del corazón (I25, I21)** ocupan las primeras posiciones, consistente con la transición epidemiológica del país hacia enfermedades crónicas no transmisibles.
- La **enfermedad pulmonar obstructiva crónica (J44)** refleja factores como contaminación, tabaquismo y uso de leña para cocción.
- Los **homicidios (X95)** aparecen entre las primeras causas, evidenciando que la violencia sigue siendo un problema de salud pública.
- La **diabetes mellitus (E14)** y la **enfermedad renal crónica (N18)** confirman la carga de enfermedades metabólicas.

---

### 6. 📊 Gráfico de Barras Apiladas — Muertes por Sexo y Departamento

**Tipo:** Gráfico de barras apiladas (hombres en azul, mujeres en rosa).

**Descripción:** Compara el total de defunciones por sexo en cada departamento, permitiendo identificar diferencias de género significativas.

**Hallazgos clave:**
- En todos los departamentos, la mortalidad masculina supera a la femenina, con una proporción aproximada de **55.7% hombres vs 44.0% mujeres**.
- La brecha de género es más pronunciada en departamentos con mayor violencia (Valle del Cauca, Antioquia), donde los homicidios afectan desproporcionadamente a hombres jóvenes.
- Los casos de sexo "indeterminado" (~0.3%) corresponden principalmente a cuerpos no identificados.

---

### 7. 📊 Histograma — Distribución por Grupo de Edad

**Tipo:** Gráfico de barras con colores diferenciados por categoría de edad.

**Descripción:** Agrupa las defunciones según los rangos de edad definidos por la variable GRUPO_EDAD1 del DANE, permitiendo identificar patrones de mortalidad a lo largo del ciclo de vida.

| Categoría | Códigos DANE | Rango de edad |
|-----------|:------------:|--------------|
| Mortalidad neonatal | 0–4 | < 1 mes |
| Mortalidad infantil | 5–6 | 1–11 meses |
| Primera infancia | 7–8 | 1–4 años |
| Niñez | 9–10 | 5–14 años |
| Adolescencia | 11 | 15–19 años |
| Juventud | 12–13 | 20–29 años |
| Adultez temprana | 14–16 | 30–44 años |
| Adultez intermedia | 17–19 | 45–59 años |
| Vejez | 20–24 | 60–84 años |
| Longevidad / Centenarios | 25–28 | 85–100+ años |
| Edad desconocida | 29 | Sin información |

**Hallazgos clave:**
- La **vejez (60-84 años)** concentra la mayor proporción de defunciones, reflejando la estructura de mortalidad esperada en un país con envejecimiento poblacional progresivo.
- La **adultez intermedia (45-59 años)** presenta un volumen significativo, impulsado por enfermedades crónicas y violencia.
- La **mortalidad neonatal e infantil** muestra valores relativamente bajos pero relevantes para políticas de salud materno-infantil.
- La **juventud y adolescencia** presentan un componente importante de muertes por causas externas (violencia y accidentes).

---

## Fuente de Datos

- **DANE — Estadísticas Vitales**: Defunciones No Fetales 2019
  - Cifras definitivas: 242,609 defunciones no fetales registradas entre el 1 de enero y el 31 de diciembre de 2019.
  - Microdatos: https://microdatos.dane.gov.co/index.php/catalog/696
  - Publicación de cifras: https://www.dane.gov.co/index.php/estadisticas-por-tema/salud/nacimientos-y-defunciones/defunciones-no-fetales/defunciones-no-fetales-2019

---

## Licencia

Proyecto académico con fines educativos. Los datos del DANE son de acceso público bajo la normativa colombiana de datos abiertos.

---

*Desarrollado con Python · Dash · Plotly | Universidad de La Salle — Maestría en Inteligencia Artificial*
