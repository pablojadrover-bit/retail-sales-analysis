# 📊 Análisis de Ventas Retail — Power BI + DAX
### Adrover Consultoría | Pablo Javier Adrover

**Herramientas:** Power BI Desktop · DAX · Power Query  
**Dataset:** 500 transacciones · 3 sucursales · 6 líneas de producto  
**Perfil:** [LinkedIn](https://www.linkedin.com/in/pablojadrover) · [Diagnóstico gratuito para PyMEs](https://pablojadrover-bit.github.io/diagnostico-pymes)

---

## 🎯 Objetivo

Identificar oportunidades de mejora en rentabilidad, comportamiento de clientes y desempeño por sucursal en una cadena minorista de electrónica con múltiples puntos de venta.

Este análisis muestra cómo pasar de datos crudos a decisiones concretas usando Power BI y DAX — sin adivinar, sin intuición.

---

## ❓ Problema de negocio

La empresa factura pero no tiene visibilidad sobre cuatro preguntas clave:

1. ¿Qué categorías generan más **margen real**, no solo más volumen?
2. ¿Cuál sucursal tiene el mejor **desempeño ajustado** por mix de productos?
3. ¿Los clientes nuevos son más o menos **rentables** que los que vuelven?
4. ¿El medio de pago tiene algún **patrón** que valga la pena explotar?

Sin estas respuestas, cualquier decisión de inventario, promoción o expansión es un juego de azar.

---

## 📊 KPIs Generales

| Métrica | Valor |
|---|---|
| 💰 Total Ventas | $88.104.804 |
| 🧾 Total Órdenes | 500 |
| 🎯 Ticket Promedio | $176.210 |
| ⭐ Rating Promedio | 69,9 / 100 |

---

## 🔍 Hallazgos principales

### 1. La categoría que más vende NO es la más rentable

| Categoría | Ventas | Ticket Promedio | Órdenes |
|---|---|---|---|
| Accesorios | $16.745.467 | $184.016 | 91 |
| Laptops | $16.020.245 | $188.473 | 85 |
| **Smartphones** | $14.490.014 | **$201.250 ↑** | 72 |
| Audio | $14.297.461 | $166.250 | 86 |
| Televisión | $13.475.325 | $153.129 | 88 |
| Gaming | $13.076.292 | $167.645 | 78 |

**➜** Smartphones tiene el **ticket más alto del catálogo** ($201.250) pero solo el tercer lugar en volumen. Es el segmento de mayor valor unitario y está siendo subestimado en la estrategia comercial.

---

### 2. Más ventas no siempre significa mejor sucursal

| Sucursal | Ventas | Órdenes | Rating |
|---|---|---|---|
| C | $31.301.346 | 165 | 67,5 ⚠️ |
| A | $29.374.611 | 168 | **71,8 ✅** |
| B | $27.428.847 | 167 | 70,2 |

**➜** La Sucursal C lidera en facturación pero tiene el **rating más bajo** (67,5 vs 71,8 de A). Ventas altas con satisfacción baja es señal clásica de crecimiento sin fidelización. Una caída de 3 puntos en rating puede anticipar una caída del 15-20% en clientes recurrentes en 6 meses.

---

### 3. Los clientes nuevos compran más que los recurrentes — y eso es una alerta

| Tipo | Género | Ventas | Órdenes | Rating |
|---|---|---|---|---|
| **Nuevo** | Masculino | $25.444.861 | 145 | 71,4 |
| Recurrente | Masculino | $21.858.943 | 124 | 68,5 |
| **Nuevo** | Femenino | $21.272.152 | 113 | 69,9 |
| Recurrente | Femenino | $19.528.848 | 118 | 69,3 |

**➜** Los clientes nuevos superan a los recurrentes en ventas y en satisfacción. Esto indica un **problema de retención**: captan bien pero no fidelizan. Conseguir un cliente nuevo cuesta 5-7x más que retener uno existente.

---

### 4. El efectivo todavía manda, la billetera digital ya lo alcanza

| Medio de Pago | Ventas | Órdenes | Participación |
|---|---|---|---|
| Efectivo | $31.431.711 | 181 | 35,7% |
| Billetera Digital | $31.345.899 | 172 | 35,6% |
| Tarjeta de Crédito | $25.327.194 | 147 | 28,7% |

**➜** Efectivo y billetera digital están casi empatados. Con un pequeño incentivo (2-3% de descuento), la billetera digital puede superar al efectivo, reduciendo costos operativos de manejo de caja.

---

## 💡 Recomendaciones

**1. Estrategia Smartphones:** El ticket más alto + volumen moderado = clientes de mayor poder adquisitivo sin plan de fidelización. Agregar garantías extendidas y accesorios complementarios puede incrementar el ticket promedio general un 8-12%.

**2. Auditoría Sucursal C:** Rating bajo + ventas altas es una combinación que no escala. Cruzar con datos de devoluciones, tiempos de atención y rotación de personal antes de que el problema se haga visible en la facturación.

**3. Programa de retención:** Si los nuevos compran más que los recurrentes, hay un ciclo de vida roto. Un descuento en segunda compra o comunicación post-venta puede revertir la tendencia en 2-3 ciclos.

**4. Migración a billetera digital:** Con 35,6% de participación ya tiene masa crítica. Un incentivo mínimo puede inclinar la balanza y reducir costos operativos de caja.

---

## 🛠️ Medidas DAX creadas

```dax
-- Ventas totales del período
Total Ventas = SUM(retail_sales_dataset[Venta Total])

-- Cantidad de transacciones
Total Órdenes = COUNTROWS(retail_sales_dataset)

-- Ticket promedio por transacción
Ticket Promedio = AVERAGE(retail_sales_dataset[Venta Total])

-- Margen bruto total
Margen Bruto Total = SUM(retail_sales_dataset[Ingreso Bruto])

-- Participación en ventas totales
% Participación Ventas =
DIVIDE(
    [Total Ventas],
    CALCULATE([Total Ventas], ALL(retail_sales_dataset)),
    0
)

-- Comparación ticket vs promedio de la categoría
Ventas vs Promedio Categoría =
VAR PromCat =
    CALCULATE(
        AVERAGE(retail_sales_dataset[Venta Total]),
        ALLEXCEPT(retail_sales_dataset, retail_sales_dataset[Categoría ES])
    )
RETURN DIVIDE([Ticket Promedio] - PromCat, PromCat, 0)

-- Satisfacción promedio del cliente
Rating Promedio = AVERAGE(retail_sales_dataset[Rating])
```

---

## 📐 Estructura del Dashboard

**Página 1 — Resumen Ejecutivo**
- 4 tarjetas KPI: Total Ventas · Órdenes · Ticket Promedio · Rating
- Gráfico de barras: Ventas por Categoría (en castellano)
- Gráfico de barras: Ventas por Sucursal
- Segmentador interactivo por Sucursal y Categoría

**Página 2 — Análisis de Clientes**
- Tabla: Tipo de Cliente × Género × Ventas × Rating
- Gráfico circular: Distribución por Medio de Pago (en castellano)

---

## 📁 Estructura del repositorio

```
retail-sales-analysis/
├── README.md                        ← documentación completa del análisis
├── prueva.pbix                      ← archivo Power BI con modelo y dashboard
└── imagenes/
    ├── dashboard-resumen.png        ← captura página 1
    └── dashboard-clientes.png      ← captura página 2
```

---

## 👤 Sobre el autor

**Pablo Javier Adrover** — Consultor en Control Financiero e Inteligencia de Negocio.  
18+ años de experiencia en administración financiera, Power BI, SQL y procesos ETL.  
Especializado en PyMEs de logística, construcción y agroindustria.

¿Tu empresa tiene datos pero no tiene respuestas? → **[Diagnóstico gratuito en 5 minutos](https://pablojadrover-bit.github.io/diagnostico-pymes)**

---
*Desarrollado con Power BI Desktop · DAX · MCP Integration · Adrover Consultoría 2026*
