# ✈ Segmentación de clientes basada en el modelo LRFMC
El proyecto se centró en aplicar Machine Learning(clúster) para construir un modelo LRFMC

- Tools : Python [View](https://github.com/litahu/Segmentaci-n-de-clientes---LRFMC/blob/main/airlines.ipynb) <br>
- Dataset: Udacity Academy 
<br>

---

## 📂 **Introducción**
### Declaración del problema 
- Junto con el crecimiento de las redes aéreas, la competencia entre las compañías aéreas es cada vez más intensa y creciente. Los problemas que suelen ocurrir en esta empresa incluyen **pérdida de clientes** y **disminución de la competitividad**. 
- Con el advenimiento de la era de la información, el enfoque de marketing de la empresa ha pasado de estar *basado en el producto* a estar ***basado en el cliente***. Un enfoque que se puede adoptar es la **clasificación o segmentación de clientes**, que permite a las aerolíneas diferenciar entre clientes de alto y bajo valor y brindar **servicio personalizado** y **estrategias de marketing** para cada grupo. El objetivo es **maximizar las ganancias** centrando los recursos en los clientes que son valiosos para la empresa.

### Objetivos
1. Segmentación de clientes utilizando datos de clientes de aerolíneas según el modelo LRFMC utilizando K-Means.<br>
2. Analizar las características de cada grupo/cluster resultante de la segmentación.<br>
3. Proporcionar información empresarial relacionada con los resultados del análisis.<br>
<br>

---

## 📂 **Análisis exploratorio de datos**
### Dataset
- El conjunto de datos tiene 62988 filas y 23 características (15 numéricas, 8 categóricas).
- Hay un 7,51% de valores faltantes (PROVINCIA_TRABAJO, CIUDAD_TRABAJO, SUM_AÑO_1, EDAD, SUM_AÑO_2, PAÍS_TRABAJO).
- No hay datos duplicados.

### Resumen
- La mayoría de las características tienen un sesgo positivo con media > mediana.
- Hay valores atípicos en todas las características numéricas.
- Hay un valor de 0 en las funciones EXCHANGE_COUNT, avg_discount, Points_Sum, Point_NotFlight, SUM_YR_1, SUM_YR_2, AVG_INTERVAL y MAX_INTERVAL.
- Hasta el 77% de los clientes de las aerolíneas son hombres.
- La mayoría, el 92% de los clientes de las aerolíneas provienen de China.
- Hasta el 29% de los clientes de la aerolínea proceden de la provincia de Guangdong y el 15% de la ciudad de Guangzhou.
<br>

---

## 📂 **Pre-proceso**
### Tratamiento de los valores que faltan
- Según las observaciones que se han realizado, existen valores anómalos, concretamente 0, en los precios de los billetes (SUM_YR_1, SUM_YR_2), avg_discount y el kilometraje total (SEG_KM_SUM) que son mayores que 0. Los datos con precios de billetes vacíos o 0 pueden deberse a por clientes que no tienen historial de viaje. Estos datos se eliminan.
- Se eliminaron características irrelevantes, de muy bajo impacto y redundantes como MEMBER_NO, WORK_CITY, WORK_PROVINCE, WORK_COUNTRY, AGE, GENDER.
- Debido a que después de este paso los valores faltantes pasaron a ser 1,1%, se eliminaron los datos restantes a los que les faltaban valores.

### Selección de características
- LRFMC es un método que se puede utilizar para segmentar a los clientes de aerolíneas. L significa lealtad, R por actualidad, F por frecuencia, M por valor monetario y C por categoría (descuento). Estos cinco factores se pueden utilizar para segmentar a los clientes en función de su lealtad a las aerolíneas, la frecuencia con la que viajan, la cantidad que gastan en boletos de avión y la cantidad de descuentos que utilizan los clientes.
- Con base en el modelo LRFMC, se seleccionan seis características relacionadas con los índices del modelo LRFMC: FFP_DATE, LOAD_TIM, FLIGHT_COUNT, AVG_DISCOUNT, SEG_KM_SUM, LAST_TO_END.<br>

El método de cálculo específico es el siguiente:

### Tratamiento de los valores atípicos

### Estandarización de características

<br>

---

## 📂 **Modeling**
