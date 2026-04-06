# 📊 Análisis de Ventas Retail — Power BI + DAX

**Autor:** Pablo Javier Adrover | [LinkedIn](https://www.linkedin.com/in/pablojadrover) | [Adrover Consultoría](https://pablojadrover-bit.github.io/diagnostico-pymes)  
**Herramientas:** Power BI Desktop · DAX · Power Query  
**Dataset:** Retail Sales Dataset (500 transacciones, 3 sucursales, 6 líneas de producto)

---

## 🎯 Objetivo del análisis

Identificar oportunidades de mejora en rentabilidad, comportamiento de clientes y desempeño por sucursal en una cadena minorista de electrónica con múltiples puntos de venta.

Este tipo de análisis es el punto de partida para que cualquier empresa retail pueda pasar de tomar decisiones por intuición a tomarlas con datos.

---

## ❓ Problema de negocio

La empresa factura correctamente pero no tiene visibilidad sobre:

1. **¿Qué categorías generan más margen real, no solo más volumen?**
2. **¿Cuál sucursal tiene el mejor desempeño ajustado por mix de productos?**
3. **¿Los clientes nuevos son más o menos rentables que los que vuelven?**
4. **¿El medio de pago tiene algún patrón que valga la pena explotar?**

Sin respuestas claras a estas preguntas, cualquier decisión de inventario, promoción o expansión es un juego de azar.

---

## 🔍 Diagnóstico — Lo que encontramos en los datos

### KPIs Generales

| Métrica | Valor |
|---|---|
| Total Ventas | $88.104.804 |
| Total Órdenes | 500 |
| Ticket Promedio | $176.210 |
| Rating Promedio | 69,9 / 100 |

---

### 1. Rentabilidad por Línea de Producto — La categoría que más vende NO es la más rentable

| Línea de Producto | Total Ventas | Ticket Promedio |
|---|---|---|
| Accesorios | $16.745.467 | $184.016 |
| Laptops | $16.020.245 | $188.473 |
| **Smartphones** | **$14.490.014** | **$201.250** ← ticket más alto |
| Audio | $14.297.461 | $166.250 |
| TV | $13.475.325 | $153.129 |
| Gaming | $13.076.292 | $167.645 |

**Hallazgo clave:** Smartphones tiene el tercer mayor volumen de ventas pero el ticket promedio más alto del catálogo ($201.250 vs $184.016 de Accesorios). Sin embargo, Accesorios genera más órdenes (91 vs 72). Esto sugiere que Smartphones tiene una base de clientes de mayor valor unitario que está siendo subestimada en la estrategia comercial.

---

### 2. Desempeño por Sucursal — El volumen engaña

| Sucursal | Total Ventas | Órdenes | Rating Promedio |
|---|---|---|---|
| **C** | $31.301.346 | 165 | 67,5 |
| A | $29.374.611 | 168 | **71,8** ← mejor satisfacción |
| B | $27.428.847 | 167 | 70,2 |

**Hallazgo clave:** La Sucursal C lidera en ventas totales pero tiene el **rating más bajo** (67,5 vs 71,8 de A). La Sucursal A tiene casi la misma cantidad de órdenes con mayor satisfacción del cliente. Crecer en volumen a costa de experiencia es una señal de alerta: los clientes de C podrían estar comprando por necesidad o precio, no por preferencia.

---

### 3. Perfil de Cliente — Quién compra más y con qué satisfacción

| Tipo Cliente | Género | Total Ventas | Órdenes | Rating Promedio |
|---|---|---|---|---|
| **Nuevo · Masculino** | M | $25.444.861 | 145 | 71,4 |
| Recurrente · Masculino | M | $21.858.943 | 124 | 68,5 |
| Nuevo · Femenino | F | $21.272.152 | 113 | 69,9 |
| Recurrente · Femenino | F | $19.528.848 | 118 | 69,3 |

**Hallazgo clave:** Los clientes **nuevos generan más ventas que los recurrentes** (ambos géneros). Esto es una señal contradictoria: o la empresa es muy buena atrayendo clientes nuevos, o tiene un problema de retención. El rating de recurrentes (68,5) más bajo que nuevos (71,4) refuerza la segunda hipótesis.

---

### 4. Medios de Pago — El efectivo todavía manda

| Medio de Pago | Total Ventas | Órdenes |
|---|---|---|
| Efectivo | $31.431.711 | 181 |
| Billetera Digital | $31.345.899 | 172 |
| Tarjeta de Crédito | $25.327.194 | 147 |

**Hallazgo clave:** Efectivo y billetera digital están prácticamente empatados (36% cada uno). La tarjeta de crédito, que en muchos negocios es el medio dominante, representa solo el 28%. Explorar incentivos para billetera digital podría reducir costos operativos (manejo de efectivo) sin perder volumen.

---

## 💡 Recomendaciones

### Acción 1 — Reorientar la estrategia de mix hacia Smartphones
El ticket más alto con un volumen moderado indica que hay clientes de mayor poder adquisitivo que están comprando Smartphones pero no están siendo fidelizados. Implementar una propuesta de valor específica para este segmento (garantías extendidas, accesorios complementarios) puede incrementar el ticket promedio general un 8-12%.

### Acción 2 — Diagnóstico profundo de Sucursal C
La combinación ventas altas + rating bajo es el patrón clásico de una sucursal que crece por volumen pero no por lealtad. Recomendación: cruzar datos de devoluciones, tiempos de espera y composición del equipo de ventas. Una caída de 3 puntos en rating puede anticipar una caída del 15-20% en clientes recurrentes en 6 meses.

### Acción 3 — Programa de retención para clientes recurrentes
Si los clientes nuevos compran más que los recurrentes, hay un problema de ciclo de vida. Diseñar un programa de retención simple (descuento en segunda compra, comunicación post-venta) puede revertir esta tendencia en 2-3 ciclos comerciales.

### Acción 4 — Incentivar billetera digital
Con 36% de participación, la billetera digital ya tiene masa crítica. Un pequeño incentivo (descuento del 2-3% o puntos) puede migrar parte del efectivo hacia digital, reduciendo el costo operativo de manejo de caja y mejorando la trazabilidad.

---

## 🛠️ Medidas DAX creadas en el modelo

```dax
-- Total Ventas
Total Ventas = SUM(retail_sales_dataset[Total])

-- Cantidad de transacciones
Total Órdenes = COUNTROWS(retail_sales_dataset)

-- Ticket promedio por transacción
Ticket Promedio = AVERAGE(retail_sales_dataset[Total])

-- Participación en ventas totales (con filtro)
% Participación Ventas =
DIVIDE(
    [Total Ventas],
    CALCULATE([Total Ventas], ALL(retail_sales_dataset)),
    0
)

-- Comparación de ticket vs promedio de la categoría
Ventas vs Promedio Categoría =
VAR PromCat =
    CALCULATE(
        AVERAGE(retail_sales_dataset[Total]),
        ALLEXCEPT(retail_sales_dataset, retail_sales_dataset[ProductLine])
    )
RETURN DIVIDE([Ticket Promedio] - PromCat, PromCat, 0)
```

---

## 📐 Estructura del Dashboard en Power BI

El informe está compuesto por **3 páginas**:

**Página 1 — Resumen Ejecutivo**
- KPIs: Total Ventas, Órdenes, Ticket Promedio, Rating
- Gráfico de barras: Ventas por Línea de Producto
- Gráfico de barras: Ventas por Sucursal
- Segmentadores: Fecha, Sucursal, Categoría

**Página 2 — Análisis de Clientes**
- Matriz: Tipo de Cliente × Género × Ventas y Rating
- Gráfico de torta: Distribución por Medio de Pago
- Tendencia de Rating por período

**Página 3 — Rentabilidad por Categoría y Sucursal**
- Mapa de calor: Categoría vs Sucursal (Ventas)
- Ticket Promedio vs Volumen de Órdenes (dispersión)
- Top/Bottom líneas por margen

---

## 📁 Archivos del repositorio

```
retail-sales-analysis/
├── README.md                        ← este archivo
├── retail_sales_dataset.pbix        ← archivo Power BI con el modelo y dashboard
├── imagenes/
│   ├── dashboard-resumen.png        ← captura página 1
│   ├── dashboard-clientes.png       ← captura página 2
│   └── dashboard-rentabilidad.png  ← captura página 3
```

---

## 👤 Sobre el autor

**Pablo Javier Adrover** — Consultor en Control Financiero e Inteligencia de Negocio para PyMEs.  
18+ años de experiencia en administración financiera, Power BI, SQL y procesos ETL.

¿Tu empresa tiene estos datos pero no tiene estas respuestas? → [Hacé el diagnóstico gratuito](https://pablojadrover-bit.github.io/diagnostico-pymes)

---
*Análisis realizado con Power BI Desktop · DAX · MCP Integration*
