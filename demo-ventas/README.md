# Demostración con dataset sintéticos de ventas

## Ficheros de datos (directorio `data`)

* fact_ventas.csv
* dim_productos.csv
* dim_clientes.csv
* dim_tiendas.csv
* dim_calendario.csv
* objetivos_mensuales.csv
* dataset_ventas_ficticio.xlsx (mismas tablas en Excel)

## Ficheros de apoyo

* medidas_dax.txt (medidas listas para copiar/pegar)

## Demostración paso a paso

### Montar el modelo (10 minutos)

1) Abrir Power BI Desktop → **Obtener datos** → (CSV o Excel).
2) Cargar estas tablas con los nombres:
   * fact_ventas
   * dim_productos
   * dim_clientes
   * dim_tiendas
   * dim_calendario
   * objetivos_mensuales

3) Crear relaciones (modelo estrella):
   * fact_ventas[ProductID]  * → 1  dim_productos[ProductID]
   * fact_ventas[CustomerID] * → 1  dim_clientes[CustomerID]
   * fact_ventas[StoreID]    * → 1  dim_tiendas[StoreID]
   * fact_ventas[OrderDate]  * → 1  dim_calendario[Date]
   * objetivos_mensuales[StoreID]   * → 1 dim_tiendas[StoreID]
   * objetivos_mensuales[YearMonth] * → 1 dim_calendario[YearMonth]  (relación por texto)

4) Marcar la tabla de fechas:
   * Modelado → **Marcar como tabla de fechas** → dim_calendario[Date]

5) Crear medidas:
   * Modelado → **Nueva medida** y pega las medidas de `medidas_dax.txt`.

### Sugerencia de páginas del informe

Página 1: Resumen

* KPI: Ventas, Margen, % Margen, Pedidos, Ticket medio
* Línea: Ventas por Date
* Barras: Top 10 productos por Ventas
* Segmentadores (sliders): YearMonth, Category, Region, Channel, Segment

Página 2: Tiendas

* Matriz: Region > City > StoreName con Ventas, Objetivo, % Cumplimiento
* Barras: Brecha a objetivo por tienda
* Tooltip: % Ventas devueltas

Página 3: Drillthrough Producto

* Tabla detalle + evolución mensual + segmentadores de tienda/cliente

## Notas

* `fact_ventas` incluye devoluciones (IsReturn=1 con importes negativos) y un pequeño % de pedidos cancelados (OrderStatus=Cancelado con importes 0).
* La moneda es EUR (puedes formatear `Ventas` como €).
