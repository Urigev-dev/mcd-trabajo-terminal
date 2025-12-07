# Modelo Predictivo de Conversión en Lanzamientos Digitales (Trabajo Terminal MCD)

Este repositorio contiene el código, los notebooks y los artefactos generados en el trabajo terminal de la Maestría en Ciencia de Datos, orientado al modelado de la probabilidad de avance de leads en campañas tipo *speed-launch* para programas ejecutivos de formación en datos de REDACERT.

El objetivo global del proyecto es estimar la probabilidad de conversión a cliente (compra) a partir de datos de marketing y comportamiento (registro → asistencia → compra). El avance actual se centra en el **submodelo de probabilidad de asistencia** a la masterclass del lanzamiento L1 (MasterData 14™). En una segunda fase se incorporará un Dataset v2, correspondiente al lanzamiento de un curso de Power BI, que permitirá modelar la probabilidad de compra.

## Estructura del repositorio

```text
mcd-trabajo-terminal/
├─ data/
│  ├─ raw/              → Datos crudos (NO versionados: exportaciones originales de Clientify)
│  └─ processed/        → Datos procesados y features (l1_clean.csv, l1_features.csv, etc.)
├─ notebooks/
│  └─ 01_L1_datos_y_features.ipynb  → Notebook principal: limpieza, features y modelado v1
├─ models/
│  └─ modelo_L1_asistencia.pkl      → Pipeline entrenado (preprocesamiento + regresión logística)
├─ docs/
│  └─ Proyecto_terminal.docx        → Documento de avance del trabajo terminal
├─ .gitignore
└─ README.md
```

> **Nota:** la carpeta `data/raw/` está excluida del control de versiones por contener datos sensibles (datos personales de leads). Para reproducir los experimentos, el usuario debe contar con su propia exportación de Clientify y colocarla en `data/raw/`.

## Requerimientos

- Python 3.10 o superior (recomendado)  
- Entorno virtual (por ejemplo, `venv`)  
- Librerías principales:
  - `pandas`
  - `numpy`
  - `scikit-learn`
  - `imbalanced-learn`
  - `matplotlib`
  - (Opcionales) `jupyter`, `seaborn`

Una forma estándar de preparar el entorno es:

1. Crear y activar un entorno virtual.  
2. Instalar las dependencias desde un archivo `requirements.txt` generado con `pip freeze`.

Ejemplo de instalación:

```bash
pip install -r requirements.txt
```

## Flujo de trabajo

1. Colocar el archivo crudo exportado desde Clientify en:

   ```text
   data/raw/clientify_contactos.csv
   ```

2. Abrir y ejecutar el notebook:

   ```text
   notebooks/01_L1_datos_y_features.ipynb
   ```

3. Ejecutar las celdas en orden para:

   - Cargar el dataset crudo desde `data/raw/`.  
   - Generar el **Dataset v1 limpio** y guardarlo en `data/processed/l1_clean.csv`.  
   - Construir el **dataset de características** y guardarlo en `data/processed/l1_features.csv`.  
   - Dividir el dataset en entrenamiento y prueba (70/30 con estratificación).  
   - Entrenar y evaluar los modelos de clasificación (regresión logística, árbol de decisión, random forest).  
   - Comparar métricas (AUC, accuracy, precision, recall, F1-score) y seleccionar el modelo ganador.  
   - Serializar el pipeline entrenado (preprocesamiento + modelo) en `models/modelo_L1_asistencia.pkl`.

4. Utilizar el modelo serializado para generar *scores* de probabilidad de asistencia sobre nuevos leads, asegurando que los datos de entrada respetan el mismo esquema de features.

## Alcance del avance

El avance documentado en este repositorio cubre:

- La construcción del **Dataset v1** a partir de la campaña piloto de MasterData 14™ (30 de julio al 10 de agosto de 2025), con 224 leads y etiqueta de asistencia (36 asistentes, 188 no asistentes).  
- El diseño y generación de un **dataset analítico de características** que integra variables de CRM, parámetros UTM y atributos temporales.  
- El entrenamiento y la comparación de tres modelos de clasificación binaria (regresión logística, árbol de decisión y random forest) para estimar la probabilidad de asistencia.  
- La selección de la **regresión logística** como modelo ganador v1 de asistencia, con un AUC ≈ 0.644 en el conjunto de prueba, y la serialización del pipeline correspondiente.

El **modelo de probabilidad de compra** no se ha entrenado todavía, debido a que en el lanzamiento L1 no se registraron compras. Este componente se desarrollará en una segunda fase, a partir del Dataset v2 del lanzamiento del curso de Power BI, donde se dispondrá de la etiqueta de compra (cliente / no cliente).

## Documento del trabajo terminal

El documento de avance del trabajo terminal asociado a este repositorio se encuentra en:

```text
docs/Proyecto_terminal.docx
```

En este archivo se describe en detalle:

- El contexto de negocio y el problema de investigación.  
- El estado del arte en lead scoring, conversión en campañas digitales y marcos metodológicos (CRISP-DM, CRISP-ML(Q), MLOps).  
- El diseño del estudio, la metodología y el pipeline de datos.  
- El modelado del submodelo de asistencia, sus resultados, discusión y conclusiones del avance.

## Buenas prácticas y consideraciones de privacidad

- Los datos crudos de Clientify no se versionan para proteger la privacidad de los leads.  
- Cualquier uso de este repositorio debe realizarse con datasets propios, respetando la normativa aplicable de protección de datos personales (GDPR, legislación local, políticas internas de la organización).  
- El código y los artefactos se proporcionan con fines académicos y de referencia para casos de uso similares en marketing educativo y lanzamientos digitales.

## Autor

- **Uriel Gerónimo Velázquez**  
  Maestría en Ciencia de Datos  
  REDACERT – Red Académica de Certificación