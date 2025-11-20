# BASE DE DATOS ACTIVIDAD – LLANTAS_DB

## DESCRIPCIÓN DEL PROYECTO

En esta práctica se desarrolló una base de datos llamada **llantas_db**, que contiene tablas relacionadas: clientes, productos, ventas y detalle_venta.
El objetivo principal fue aplicar conocimientos de bases de datos relacionales como consultas, subconsultas, índices, vistas, transacciones y procedimientos almacenados, midiendo además el rendimiento de las consultas y la eficiencia de los índices.
Todo se documentó y se organizó para que sea fácil de entender y seguir paso a paso.

## INVESTIGACIÓN

### CREATE DATABASE

Se utilizó la sentencia:
CREATE DATABASE llantas_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

* **CHARACTER SET:** define la codificación de los caracteres que se pueden almacenar en la base de datos. Permite letras, números, símbolos y emojis.
* **COLLATE:** define cómo se comparan y ordenan los caracteres. Por ejemplo, si distingue entre mayúsculas, minúsculas y acentos.
* **utf8mb4:** permite almacenar emojis y caracteres universales, porque usa hasta 4 bytes por carácter.
* **Case-insensitive:** significa que no distingue entre mayúsculas y minúsculas al comparar texto.

Esto afecta las consultas como:

* **LIKE:** no distingue mayúsculas/minúsculas.
* **ORDER BY:** el orden de palabras con acento o mayúsculas puede variar.
* **GROUP BY:** agrupa palabras según las reglas de collation; palabras iguales con distinto acento o mayúscula pueden considerarse diferentes.


### ENGINE = InnoDB

* InnoDB es un motor de almacenamiento confiable y moderno que soporta transacciones, llaves foráneas y ACID.
* Se usa para garantizar la integridad de los datos, evitar pérdidas y manejar correctamente errores en operaciones múltiples.
* Se coloca al final de la definición de la tabla: ENGINE=InnoDB.
* Ventajas: permite transacciones, llaves foráneas, bloqueo a nivel de fila y cumplimiento ACID.

Ejemplo práctico: si intentamos crear un pedido para un cliente que no existe, InnoDB rechaza la operación gracias a la llave foránea.


### DATOS HUÉRFANOS

* Un dato huérfano es un registro hijo que apunta a un padre que ya no existe.
* Esto ocurre al eliminar un registro padre sin eliminar o actualizar sus hijos.
* Es un problema porque provoca errores en las consultas, resultados incompletos y afecta la integridad de la base.
* Se evita usando llaves foráneas con reglas como **ON DELETE CASCADE**, **SET NULL** o **RESTRICT**.


### INTEGRIDAD REFERENCIAL

* Garantiza que las relaciones entre tablas sean coherentes.
* Se aplica usando claves primarias en la tabla padre y claves foráneas en la tabla hija.
* Ejemplo: cada pedido debe tener un cliente válido.
* Acciones referenciales:

  * **CASCADE:** elimina o actualiza hijos automáticamente.
  * **RESTRICT:** impide cambios si hay hijos.
  * **SET NULL:** pone la FK en NULL.
  * **SET DEFAULT:** pone un valor por defecto.
  * **NO ACTION:** impide cambios que rompan la integridad.
<img width="863" height="354" alt="image" src="https://github.com/user-attachments/assets/d62400e9-81fd-48e8-833d-d01d2d8cc0d6" />


### ÍNDICES

* Un índice ayuda a encontrar registros rápidamente sin revisar toda la tabla.
* Problemas sin índices: consultas lentas, full table scan, mayor consumo de CPU.
* Tipos: normal, único, compuesto, fulltext, hash, espacial.
* Se recomienda crear índices en columnas que se usan mucho en WHERE, JOIN, ORDER BY o GROUP BY.


### DECLARE Y SET EN PROCEDIMIENTOS

* DECLARE se usa para crear variables internas en un procedimiento.
* SET se usa para asignar valores o modificar variables dentro del procedimiento.
* Ejemplo:
  
DECLARE i INT DEFAULT 1;
SET i = i + 1

## PARTE PRÁCTICA

### CREACIÓN DE BASE DE DATOS Y TABLAS

* Se crearon las tablas clientes, productos, ventas y detalle_venta con claves primarias y foráneas correctamente definidas.
* Se insertaron entre 80 y 150 registros de prueba en cada tabla usando consultas con `WITH RECURSIVE`.


### CONSULTAS BÁSICAS

* Se hicieron consultas con **WHERE**, **LIKE**, **ORDER BY**, **GROUP BY** y funciones agregadas como **SUM** y **COUNT**.


### SUBCONSULTAS

* Se realizaron subconsultas en **WHERE**, **FROM** y subconsultas anidadas con funciones agregadas.


### ÍNDICES

* Se crearon índices en columnas importantes como `id_cliente` y `id_producto` para acelerar búsquedas.
* Se midió el rendimiento antes y después de aplicar los índices, observando mejoras significativas en las consultas.


### EVALUACIÓN DE RENDIMIENTO

* Se seleccionaron consultas lentas y se ejecutaron sin índice.
* Luego se crearon índices y se volvieron a ejecutar.
* Resultado: las consultas fueron mucho más rápidas y eficientes, especialmente en tablas grandes.


### VISTAS

* Se crearon vistas para:

  1. Consultar cuánto ha gastado cada cliente.
  2. Ver todas las ventas con detalle de productos y clientes.
  3. Identificar productos más vendidos.
* Esto simplifica la generación de reportes y facilita el análisis de datos sin repetir joins y sumas.


### TRANSACCIONES

* Se realizaron transacciones con **START TRANSACTION**, **SAVEPOINT**, **ROLLBACK** y **COMMIT**.
* Ejemplo: se intentó eliminar un cliente con ventas. Se borraron las ventas, pero se hizo rollback para no eliminar al cliente, manteniendo la integridad de la base.

### PROCEDIMIENTOS ALMACENADOS

* Se crearon procedimientos con parámetros de entrada, variables internas y lógica propia.
* Permiten automatizar tareas frecuentes como insertar detalle de venta o calcular totales.

## CONCLUSIONES FINALES

En el desarrollo de esta práctica se aplicaron diversos elementos fundamentales del trabajo con bases de datos. Primero, se comprobó que las consultas pueden guardarse de forma eficiente utilizando vistas y procedimientos almacenados, lo que permite reutilizar lógica compleja sin necesidad de escribirla nuevamente. Esto mejora la organización del proyecto y facilita el mantenimiento del código SQL.
El rendimiento de las consultas mostró una mejora notable al aplicar índices. Sin índices, las búsquedas requieren recorrer toda la tabla, generando tiempos más altos y utilizando más recursos. Al crear índices en columnas clave utilizadas frecuentemente en filtros o uniones, las consultas se optimizaron, logrando respuestas significativamente más rápidas. Esto evidencia la importancia de analizar qué columnas necesitan un índice para maximizar la eficiencia sin sobrecargar la base.

Las vistas demostraron ser herramientas muy útiles en escenarios donde se requieren reportes o consultas complejas. Permiten simplificar el acceso a la información combinando datos de varias tablas en una sola estructura lógica, manteniendo la seguridad y sin duplicar información. Además, facilitan la lectura y reducen errores al momento de consultar datos repetidamente.
Las transacciones resaltaron su importancia al permitir ejecutar varias operaciones como una sola unidad de trabajo. Gracias a los savepoints y rollback, es posible revertir cambios en caso de errores, garantizando la integridad y consistencia de la información. Esto es esencial especialmente en procesos como ventas, pagos o movimientos de inventario, donde un fallo puede afectar múltiples registros.

Finalmente, los procedimientos almacenados permitieron automatizar acciones específicas dentro de la base de datos. Gracias a sus parámetros, variables internas y lógica propia, funcionan como pequeñas funciones que estandarizan operaciones frecuentes. Su uso contribuye a mantener un sistema más organizado y fácil de modificar o ampliar



## EVIDENCIAS
<img width="948" height="643" alt="image" src="https://github.com/user-attachments/assets/b49b8f4c-941e-45ab-b2bf-6fa7db201a98" />
<img width="954" height="775" alt="image" src="https://github.com/user-attachments/assets/59d61df5-dddd-4926-ad1a-97444d4cdb10" />
