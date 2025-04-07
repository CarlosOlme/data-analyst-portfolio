# **Sales Report**

## **Objetivo**

Mostrar un resumen anual y mensual de las compras, ventas, beneficios y unidades vendidas de la empresa.
Analizar los beneficios y las ventas:
- **Por mercado/país**: Detectar la rentabilidad de cada área y posibilidades de negocio
- **Por categoría/sub-categoría**: Localizar sectores mejorables y adaptar las campañas de marketing según los intereses
**Comparación** año a año de las ventas, costes, beneficio y unidades vendidas
Análisis detallado del **crecimiento** de la empresa para detectar mejoras y deterioro en mercados y productos.

## **Dashboard**



## **Herramientas**
- **Power Query**: Transformación y limpieza de datos
- **DAX**: Medidas como `Margin = DIVIDE([SUM_Profit],[SUM_Ventas],0)`
- **Excel y Python**: Integración de de los datos

## **Pasos Clave**

### **1. Limpieza de Datos (Power Query)**
- Cambié los tipos de datos de las columnas ventas, beneficio y coste de envío mediante la configuración regional de moneda a dólar (base de datos estadounidense)
- Cambié los tipos de datos de Order date y Ship date de texto a fecha usando configuración regional estadounidense
- Creé la columna calculada coste a partir de la resta de beneficio y ventas
- Eliminé columnas con datos innecesarios (código postal)

### **2. Modelado de Datos**
- **Relaciones**: Establecí una relación 1:* entre "DateTable" y "Global-Superstore[Order Date]"
- **Jerarquías**: Creé una jerarquía "Date Jerarquía": Año -> Mes

### **3. Medidas DAX (Análisis Avanzado)**
```dax
Margin = DIVIDE([SUM_Profit],[SUM_Ventas],0)

-Objetivo: Obtener el porcentaje de beneficio respecto al coste

Profit PY = CALCULATE(SUM('Global-Superstore'[Profit]), SAMEPERIODLASTYEAR('DateTable'[Date]))

-Objetivo: Calcular el beneficio del año anterior (también creada para "Cost", "Sales" y "Quantity"

Profit Growth % Improved = 
VAR ProfitCurrent = SUM('Global-Superstore'[Profit])
VAR ProfitPrevious = [Profit PY]
VAR Growth = DIVIDE(ProfitCurrent - ProfitPrevious, ProfitPrevious, BLANK())
RETURN
    IF(
        ISBLANK(ProfitCurrent) || ISBLANK(ProfitPrevious),
        BLANK(),
        IF(
            Growth >= 0,
            FORMAT(Growth, "+0.0%"),  
            FORMAT(Growth, "-0.0%")   
        )
    )
    
-Objetivo: Mostrar crecimiento interanual con formato +/- (también implementado para "Cost", "Sales" y "Quantity")

### **4. Visualización**

-Summary (Página 1): Visión general con KPIs (Ventas, margen, costes, unidades) + comparativa interanual
-Products (Página 2): Análisis por categorías/sub-categorías (ventas, margen, costes)
-Logistics (Página 3): Análisis por mercados/países + ventas por prioridad de pedido

# **Interfaz Avanzada**

Botones para selección de año
Índice accesible
Menú desplegable para filtrado de gráficos

### **5. Insights**

**Crecimiento Anual**

-Mayor crecimiento en 2013: +32.4% beneficios vs 2012
-En 2014 (último registro):
  -Beneficios: +23.9% vs 2013
  -Ventas: +26.6% vs 2013
-Tendencia: Crecimiento anual consistente

**Mercados**

Top 2014:
1. APAC: 
2. EU: 

-Mayor margen: Canadá (aunque con menores ventas)

**Productos**

-Problema: Subcategoría "Tables" (Furniture) con 12.5% pérdidas (-$30,546)

**Top performer:**

-Categoría "Technology" ($234,928 beneficio)
-Subcategoría "Copiers" ($104,049 beneficio, 18.9% margen)
