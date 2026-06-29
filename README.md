# TP Final — Bases de Datos II

Trabajo Práctico Final de la materia **Bases de Datos II** — Tecnicatura Universitaria en Inteligencia Artificial (TUIA), FCEIA · UNR · 2026.

## Caso de negocio

**The Drinking Company (TDC)** es una empresa que produce y comercializa bebidas (colas, cervezas, gaseosas, jugos y energizantes) tanto en el mercado minorista como mayorista, con presencia en cuatro regiones de los Estados Unidos (Este, Oeste, Central y Sur). Ante la creciente demanda de análisis y la carga que las consultas generaban sobre la base operacional, la empresa decidió construir un **Data Warehouse** que centralice la información de sus distintas áreas y sirva de base para reportes de gestión en Power BI.

## Objetivo

Diseñar e implementar un Data Warehouse a partir de fuentes de datos heterogéneas (SQL Server, MySQL, XML, Excel y archivos de texto plano), con su correspondiente proceso ETL y un tablero de reportes en Power BI que responda a las consultas requeridas por las gerencias de la empresa.

## Fuentes de datos

| Área | Datos | Formato |
|---|---|---|
| Recursos Humanos | Empleados y feriados | Excel (.xls) |
| Comercialización | Clientes minoristas y mayoristas | XML |
| Comercialización | Ventas históricas (hasta 2008) | SQL Server 2000 (.bak) |
| Comercialización | Ventas actuales (desde 2009) | MySQL (.sql) |
| Comercialización | Regiones geográficas | Texto plano (.txt) |
| Producción | Productos y movimientos de stock | Texto plano (.txt) |

## Modelo dimensional

El esquema estrella (Star Schema) diseñado para este Data Warehouse consta de dos tablas de hechos y cinco dimensiones:

**Tablas de hechos**
- `FACT_VENTAS` — registra cada línea de venta con cantidad, precio unitario, precio bruto, descuento aplicado, precio final y litros consumidos. Incluye además atributos calculados como edad del cliente, edad del empleado, antigüedad del empleado y grupo etario.
- `FACT_STOCK` — registra los movimientos de stock por producto y fecha.

**Dimensiones**
- `DIM_TIME` — fecha completa con día, nombre del día, mes, nombre del mes, año, cuatrimestre, semestre, indicadores de feriado y fin de semana.
- `DIM_CUSTOMER` — cliente unificado (minorista y mayorista) con tipo, nombre, fecha de nacimiento, ciudad, estado y código postal.
- `DIM_EMPLOYEE` — empleado con género, categoría, fecha de ingreso, fecha de nacimiento y nivel educativo.
- `DIM_PROD` — producto con nombre, categoría, presentación, capacidad en ml, indicador de versión diet e indicador de discontinuado.
- `DIM_GEO` — geografía con región, estado y código postal.
- `DIM_DISCOUNT` — descuentos disponibles con monto mínimo de factura, porcentaje y rango de vigencia.

![Modelo Dimensional](./Modelo%20TP%20version%204_final.png)

## Proceso ETL

El proceso de extracción, transformación y carga fue implementado con **SQL Server Integration Services (SSIS)** en Visual Studio. El paquete ETL (`Package.dtsx`) integra las seis fuentes de datos, aplica las transformaciones necesarias (corrección de encoding en los XML, cálculo de atributos derivados como litros consumidos y grupos etarios, resolución de la lógica de descuentos) y carga las tablas del Data Warehouse.

El proyecto SSIS compilado se incluye en el repositorio como archivo `.ispac`.

## Reportes en Power BI

El tablero construido en Power BI (`TDC_TPBBDD2.pbix`) responde a los siguientes requerimientos de las gerencias:

- Cantidad de litros consumidos y productos adquiridos por cliente a lo largo del tiempo.
- Compra promedio en litros por cliente.
- Ranking de productos más vendidos por zona geográfica y período.
- Proyección del monto de ventas para el primer trimestre de 2011.
- Análisis estacional de ventas de bebidas energizantes (validación de hipótesis de marketing).
- Relación entre edad del cliente, tipo de bebida y litros vendidos.
- Segmentación de ventas por grupos etarios (adolescentes, adultos medios y edad específica).
- Relación entre características del empleado (edad y sexo) y monto de ventas.
- Proyección de los 5 vendedores más prometedores para 2011.
- Relación entre antigüedad del empleado y cantidad de productos vendidos.
- Clientes minoristas más valiosos (principio de Pareto).
- Tendencia de popularidad de bebidas Diet y bebidas en lata.
- Identificación de productos a discontinuar.
- Evolución del stock por producto y rubro.
- Clientes más valiosos y su comportamiento a lo largo del tiempo.

## Estructura del repositorio

```
TP-Final-BDDII/
├── ETL_TDC_DataWarehouse/              # Proyecto SSIS (Visual Studio)
│   └── Integration Services Project1/
│       └── Package.dtsx                # Paquete ETL principal
├── Fuentes de Datos TP Final/          # Datos de origen
│   ├── Area Comercializacion/          # XML de clientes, ventas SQL Server y MySQL
│   ├── Area Produccion/                # Productos y stock (.txt)
│   └── Area Recursos Humanos/          # Empleados y feriados (.xls / .csv)
├── Enunciado y Documentación Adicional/
├── TDC_TPBBDD2.pbix                    # Tablero Power BI
├── TDC_DataWarehouse_NOCAMBIARNOMBRE.7z # Backup comprimido del DW
└── modelo-dimensional.png              # Diagrama del modelo dimensional
```

## Stack tecnológico

- **SQL Server** — base de datos del Data Warehouse y fuente de ventas históricas
- **MySQL** — fuente de ventas actuales (2009 en adelante)
- **SSIS (SQL Server Integration Services)** — proceso ETL
- **Visual Studio** — entorno de desarrollo del proyecto SSIS
- **Power BI** — tablero de reportes y visualizaciones

## Integrantes del grupo

- FRANK, Maximiliano
- WINTER, Federico

Trabajo realizado en grupo como parte de la cursada 2026.
