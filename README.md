# Documentación Técnica: Tablero de Control - Indicadores CASE (2021-2024)

## Proyecto: Análisis y Justificación de Contratos de Administración del Servicio Educativo (CASE)

Este repositorio/documento contiene la documentación técnica del tablero de control desarrollado en Power BI para la Secretaría de Educación del Distrito (SED), diseñado para monitorear el comportamiento de los 35 colegios bajo administración.

---

## 1. Objetivo del Tablero
    Facilitar una lectura clara, dinámica y consolidada de los indicadores de calidad educativa. La herramienta sirve como sustento técnico para la justificación de los nuevos contratos CASE, evaluando el desempeño histórico (2021-2024) por institución, localidad y vigencia.

## 2. Origen de Datos
    - **Fuente:** Archivo Excel "FormatoHistoricoIndicadores_CASE 11122025" alojado en SharePoint.
    - **Estructura:** Datos normalizados en la hoja "Data" con un formato de tabla larga (Long Format).
    - **Conectividad:** Conexión vía Web URL (Direct Link) con autenticación de cuenta organizativa (OAuth2).

## 3. Procesamiento de Datos (Power Query)
    Se realizaron las siguientes transformaciones para garantizar la integridad del modelo:
    - **Limpieza de Tipos:** Conversión de vigencias a números enteros.
    - **Columnas Numéricas Auxiliares:**
        - `Resultado_Numerico`: Extracción de valores decimales mediante lógica `try Number.From([Resultado]) otherwise null` para aislar los porcentajes de las letras de Saber 11.
        - `Meta_Numerica`: Lógica similar aplicada a las metas institucionales.
    - **Normalización de Texto:** Uso de funciones `TRIM` para eliminar espacios en blanco en campos de Estado (Cumple/No Cumple).

## 4. Lógica de Negocio y DAX
    El tablero implementa medidas avanzadas para manejar la dualidad de datos (texto/número):

    ### A. Medida Principal: `Resultado_Matriz_CASE`
    Fusión de lógica visual que permite mostrar en una misma celda:
    - Porcentajes formateados al 0.00%.
    - Calificaciones de Saber 11 (A+, A, B, C, D).
    - Mensajes de "Sin información".

    ### B. Regla Global de Cumplimiento (60%)
    Lógica dinámica en la columna de totalización:
    - **Cálculo del Denominador:** Cuenta únicamente los indicadores con datos reales en la vigencia seleccionada (evitando castigar colegios por indicadores bianuales no medidos).
    - **Umbral de Aprobación:** Determina el estado "Cumple" o "No Cumple" a nivel general si la institución alcanza el 60% de sus indicadores individuales en estado "Cumple".

    ### C. Formato Condicional
    Medida `Color_Semaforo_CASE` que evalúa el estado de cumplimiento para pintar los fondos de las celdas:
    - **Verde (#C6EFCE):** Cumplimiento de meta.
    - **Rojo (#FFC7CE):** Incumplimiento de meta.

## 5. Diseño de Interfaz (UX)
    Siguiendo los lineamientos de la Dirección de Cobertura y Subsecretaría:
    - **Página 1 (Vista General):**
        - Slicer de Localidad vertical (ordenado numéricamente).
        - Slicer de Vigencia horizontal tipo mosaico (botones).
        - Matriz gerencial expandida en el área principal.
    - **Página 2 (Detalle por IED):**
        - Gráficos de barras comparativos Meta vs. Resultado.
        - **Control de Visibilidad:** La tabla de detalle solo se despliega tras seleccionar un indicador específico para evitar ruido visual.

## 6. Publicación y Mantenimiento
    - **Entorno:** Power BI Service (Web).
    - **Configuración Regional:** Ajustado a Español (Colombia) para garantizar la correcta lectura de separadores decimales (comas) en la nube.
    - **Actualización:** Programada vía Gateway de SharePoint.

## Vista General del Tablero
![Vista Principal](images/Pagina1.jpg)
![Vista Principal](images/Pagina2.jpg)

---
**Desarrollado por:** Carlos Eduardo Clavijo Tovar
**Fecha de Entrega:** Mayo 2026
**Institución:** Secretaría de Educación del Distrito (SED)
