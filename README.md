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

<p align="center">
   <kbd><img src="[Recursos/0.png](https://github.com/litahu/Segmentaci-n-de-clientes---LRFMC/blob/main/Recursos/0.JPG)" width=650px> </kbd> <br>
  Tabla 1: Cálculo de características basado en LRFMC <br>
</p>

### Tratamiento de los valores atípicos
- El manejo de los valores atípicos se realiza mediante el método **IQR**.
<p align="center">
   <kbd><img src="recursos/1.png" width=650px> </kbd> <br>
   Figura 1: Distribución de características de LRFMC después de eliminar valores atípicos  <br>
</p>

### Estandarización de características
- Estandarización usando **StandardScaler**.
<p align="center">
   <kbd><img src="recursos/2.png" width=650px> </kbd> <br>
   Figura 2: Distribución de características de LRFMC después de la estandarización <br>
</p>
<br>

---

## 📂 **Modeling**
### Encontrar K óptimo
El algoritmo K-Means es un método de agrupación basado en **centroide** (centro de agrupación). Ingrese el número de agrupaciones K y una base de datos que contenga N objetos de datos, y genere los K-clusters que cumplan con el estándar mínimo *suma de cuadrados de error*. <br>

Para determinar el número óptimo de grupos en el conjunto de datos, se realizó un análisis de grupos K utilizando el **Método del codo** y el **Gráfico de silueta**.
<p align="center">
   <kbd><img src="recursos/3.png" width=650px> </kbd> <br>
   Figura 3: Gráfico del método del codo con puntuación de distorsión <br>
</p>

Con base en los resultados del gráfico del **Método del Codo**, se observa que existen fracturas no muy agudas y una disminución significativa en los valores de inercia. Sin embargo, la línea **puntuación de distorsión** muestra que la K óptima está en **5**.

<p align="center">
   <kbd><img src="recursos/4.png" width=650px> </kbd> <br>
   Figura 4: Gráfico de trazado de silueta <br>
</p>

Según los resultados de **Gráfico de silueta**, muestra Óptimo **5**. Para determinar el valor K óptimo en el **Gráfico de silueta**, puede considerar dos factores: el coeficiente promedio lo más grande posible, pero aún más pequeño que la puntuación máxima de cada miembro del grupo, y considerar el grosor de los grupos que son similares. el uno al otro. El espesor de este cúmulo indica una composición equilibrada.<br>

### Result
Después de encontrar la K óptima, *ajustar el modelo K-Means* con **n_clusters=5** y realizar **reducción de dimensionalidad** usando **PCA**. Los resultados de los clusters formados se pueden observar en el siguiente gráfico:

<p align="center">
   <kbd><img src="recursos/5.png" width=650px> </kbd> <br>
   Figura 5: Agrupación de la segmentación de clientes <br>
</p>
<br>

---

## 📂 **Interpretación**
### Presentación de Clientes
<p align="center">
   <kbd><img src="recursos/6.png" width=650px> </kbd> <br>
   Figura 6: Porcentaje del total de clientes para cada grupo <br>
</p>

De los resultados del gráfico, se puede ver que el porcentaje **más alto** de clientes está en el **clúster 3**, es decir, **25,96 %** y el **más bajo** está en el **clúster 0**. , **16,10%** .

### Análisis de las características del clúster basado en LRFMC
<p align="center">
   <kbd><img src="recursos/7.png" width=650px> </kbd> <br>
   Figura 7: Patrones y características de los conglomerados basados ​​en LRFMC <br>
</p>

<p align="center">
   <kbd><img src="recursos/8.png" width=650px> </kbd>
  Tabla 2: Evaluación y análisis de las características del cluster <br>
</p>

**Interpretación :** <br>
1. Clúster 0 - **Hibernando**
    - Un grupo de clientes que son miembros desde hace un período de tiempo medio pero que no utilizan con frecuencia la aerolínea, tienen baja frecuencia y valores monetarios y alta antigüedad.<br>
    <br>
    
2. Grupo 1: **Clientes leales**
    - El grupo de clientes que han sido miembros durante más tiempo y tienen una actividad de vuelo moderada, el lapso de tiempo para volar no es demasiado grande y utilizan la aerolínea con bastante frecuencia.<br>
    <br>

3. Grupo 2 - **Leales potenciales - Los Campeones**
    - Los grupos de clientes que tienen una actividad de vuelos muy elevada, suelen utilizar aerolíneas y viajan largas distancias, por lo que tienen potencial para generar ingresos. Este grupo también tiene una tasa de antigüedad baja, lo que significa que el lapso de tiempo para cada vuelo no es demasiado largo ni demasiado largo. Además, el cliente es miembro desde hace bastante tiempo. <br>
    <br>
    
4. Grupo 3: **Usuario reciente**
    - Nuevos grupos de clientes que han utilizado recientemente la aerolínea. Esto se puede ver en el momento en que se unió como miembro recientemente y la tasa de actualidad es baja, aparte de que la actividad suele utilizar aerolíneas y la distancia recorrida es moderada. <br>
    <br>
    
5. Grupo 4: **Necesita atención**
    - Nuevo grupo de clientes que tienen baja actividad y uso de aerolíneas. Este grupo también tiene una tasa de descuento baja.<br>
    <br>

## 📂 **Recomendaciones para el negocio**

1. Clúster 0 - **Hibernando**
    - Clientes existentes, pero que no han utilizado la aerolínea recientemente. Se necesita tratamiento para que los clientes realicen compras lo antes posible, o la empresa perderá la confianza del cliente.
    - **Recomendaciones comerciales:**
         - Envíe correos electrónicos de marketing a clientes de este grupo con el programa "We Miss You" y proporcione vales especiales o códigos de descuento para usar en próximos vuelos con un período de validez predeterminado.<br>
<br>

2. Grupo 1: **Cliente leal**
    - Grupos de clientes que llevan mucho tiempo utilizando la aerolínea. Los clientes están satisfechos con los servicios prestados y no cambian a otras alternativas. Es importante brindar un trato para que los clientes se sientan apreciados.
    - **Recomendaciones comerciales:**
         - Envíe un correo electrónico de agradecimiento "Gracias por volar con nosotros" y proporcione un cupón/código de descuento para su próximo vuelo.
         - Proporcione puntos/recompensas por cada reserva de aerolínea que se puedan canjear por un cupón de descuento o un producto afiliado con la aerolínea.<br>
<br>

3. Grupo 2 - **Leales potenciales - Los Campeones**
    - Utilizan a menudo líneas aéreas y en largas distancias. Puede contribuir significativamente a los ingresos de la empresa. Los clientes de este grupo deben ser tratados con amabilidad y cuidado, y es necesario hacer que los clientes se sientan apreciados para que se conviertan en leales a la empresa.
     - **Recomendaciones comerciales:**
         - Construir buenas relaciones con los clientes a través del soporte de embarque, como proporcionar un asistente de reserva de vuelos.
         - Proporcionar souvenirs o mercancías.
         - Ofrecer descuentos por comprar más de un vuelo a la vez.
         - Proporcionar descuentos/recompensas especiales al volar invitando a amigos.
         - Proporcionar puntos/recompensas por cada reserva de aerolínea.<br>
<br>

4. Grupo 3: **Usuario reciente**
    - Los grupos que son nuevos en el uso de aerolíneas deben recibir tratamiento para convertirse en clientes leales a largo plazo. Es necesario realizar un seguimiento continuo para evitar que los clientes se vayan después de un determinado periodo de tiempo.
    - **Recomendaciones comerciales:**
         - Envíe un correo electrónico de agradecimiento "Gracias por volar con nosotros" y proporcione un cupón/código de descuento para su próximo vuelo.
         - Da puntos por cada vuelo.
         - Proporcionar recompensas/vales/descuentos después de lograr varios vuelos en un período determinado, por ejemplo 2 vuelos en 1 año.<br>
<br>

5. Grupo 4: **Necesita atención**
    - Nuevos grupos de clientes con muy bajo consumo, esto puede ocurrir por diversos motivos. Requiere un tratamiento personalizado según la demografía y los hábitos del cliente.
     - **Recomendaciones comerciales:**
         - Enviar campañas o promos personalizadas.
         - Envíe boletines informativos para anunciar descuentos y programas de vuelos útiles para animar a los clientes a utilizar la aerolínea nuevamente.<br>
         <br>

---
## 📂 **Referencias**

[1] Buckland T.(2024). Segmentos RFM basados ​​en análisis RFM: una guía detallada. Moengage. https://www.moengage.com/blog/rfm-analysis-using-rfm-segments/<br>

[2] Makhija, P.(2024). What is RFM Analysis? Calculating RFM Score for Customer Segmentation. CleverTap. https://clevertap.com/blog/rfm-analysis/<br>

[3] Tao, Y.(2020). Analysis Method for Customer Value of Aviation Big Data Based on LFRMC Model ICPCSEE,https://www.semanticscholar.org/paper/Analysis-Method-for-Customer-Value-of-Aviation-Big-Tao/213ccbc6ffae1c5d71eb71dea4c6fb29bcd2e675<br>

[4] Wang, P. & Chen, T.(2022). Data Value Mining: A Case Study of Airline Customer Data. IJRES. https://www.ijres.org/papers/Volume-10/Issue-4/Ser-5/B10040513.pdf<br>







