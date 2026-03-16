<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición

## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En lenguajes procedimentales como C, la composición se establece mediante la anidación de estructuras lógicas de datos. Esto permite modelar relaciones donde un elemento más complejo está formado por elementos más simples, lo que habitualmente se denomina una relación de tipo "tiene-un" o "tiene-varios".

Al utilizar este enfoque, se consigue agrupar datos lógicamente relacionados bajo una misma entidad de forma jerárquica. Por ejemplo, al definir una figura geométrica como una línea, resulta natural establecer que está compuesta internamente por puntos, delegando en la estructura del punto el almacenamiento de las coordenadas individuales.

La limitación principal de este paradigma, sin embargo, es que estas estructuras solo agrupan datos estáticos, dejando el comportamiento (las funciones operativas que calculan distancias o longitudes) separado conceptualmente de los mismos.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

### Respuesta

Al trasladar este concepto a la orientación a objetos en Java, la composición se implementa mediante clases que contienen referencias a otras clases como atributos. A diferencia de las estructuras tradicionales, las clases permiten agrupar tanto el estado (los datos) como el comportamiento (los métodos) en una única entidad cohesiva.

La principal ventaja respecto a enfoques no orientados a objetos radica en la capacidad de aplicar la ocultación de información o encapsulación. Mediante el uso de modificadores de acceso restrictivos y modificadores de inmutabilidad, se puede blindar la estructura interna de los objetos.

De esta forma, se garantiza que, una vez instanciado un objeto a través de su constructor, ni sus referencias ni su estado interno puedan ser alterados desde el exterior. Esto protege la integridad matemática y lógica de la composición a lo largo de todo su ciclo de vida.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad en el diseño orientado a objetos define el número de instancias de una clase que pueden o deben estar asociadas a una única instancia de otra clase en un momento determinado. Es un concepto fundamental para entender la cardinalidad de las relaciones estructurales entre los objetos de un sistema.

En un modelo geométrico donde una línea está formada por puntos, la relación desde la línea hacia el punto tiene una multiplicidad exacta de dos, ya que por definición geométrica se requieren siempre dos coordenadas distintas para poder trazarla.

Analizando la relación en sentido inverso, desde el punto hacia la línea, la multiplicidad dependerá de las restricciones del diseño elegido. Si los puntos se conciben como componentes exclusivos de una única línea, la multiplicidad será de uno. Por el contrario, si el sistema permite que múltiples líneas compartan la misma referencia a un punto en memoria para representar cruces o vértices, la multiplicidad sería de cero a muchos.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte, habitualmente denominada de forma estricta simplemente como "composición", implica que el objeto contenedor ejerce una propiedad exclusiva sobre sus partes. En este escenario, el ciclo de vida de los componentes está estrictamente ligado al del contenedor; si el contenedor es destruido, sus componentes pierden su razón de ser y desaparecen forzosamente junto con él.

Por otro lado, la composición débil establece una relación donde el contenedor agrupa o utiliza a otras entidades sin ser su propietario exclusivo. En este caso, los ciclos de vida son totalmente independientes, permitiendo que las partes sobrevivan en el sistema aunque el objeto agrupador principal sea eliminado de la memoria.

A este segundo modelo, más flexible en cuanto a la persistencia de las entidades, se le suele referir formalmente en el diseño de software como "agregación" o "asociación".

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase requiere interactuar con otra únicamente a través de parámetros de entrada, valores de retorno, o mediante la instanciación de objetos en variables locales dentro de la ejecución de un método, se establece una relación conocida estrictamente como "dependencia" o relación de uso.

Este tipo de interacción no se considera composición bajo ningún concepto, porque el objeto referenciado no forma parte del estado interno ni de los atributos persistentes de la clase que lo utiliza.

La relación es puramente transitoria y funcional, limitándose al contexto temporal de una operación específica. El vínculo lógico y referencial desaparece en el instante en que dicha tarea computacional ha concluido.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

La implementación teórica de la composición fuerte requiere que el objeto contenedor asuma la responsabilidad directa e indelegable de instanciar a sus componentes internos. Esto se realiza habitualmente dentro del constructor del contenedor, recibiendo únicamente los datos atómicos necesarios para la creación de las partes, lo cual garantiza que ninguna otra entidad externa posea referencias a ellas y se asegure el control total sobre su ciclo de vida.

Para implementar una composición débil, el contenedor adopta un papel pasivo en la creación. No se encarga de crear los objetos, sino que recibe las referencias a objetos complejos que ya fueron instanciados previamente en otra parte del sistema.

Al inyectar estas dependencias a través del constructor o de métodos modificadores, se evidencia y se garantiza que las partes poseen un origen externo y, por ende, una existencia totalmente independiente a la del contenedor.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

A diferencia de lenguajes de más bajo nivel (como C o C++) donde la gestión de memoria exige la destrucción explícita de los objetos mediante instrucciones manuales, el ecosistema de Java delega esta delicada responsabilidad técnica en un mecanismo automático de la máquina virtual conocido como recolector de basura.

Cuando un objeto contenedor deja de poseer referencias activas en el flujo de ejecución del programa, se vuelve elegible para ser eliminado de la memoria de forma automática. Al desaparecer el contenedor, las referencias internas que este mantenía hacia sus componentes desaparecen inevitablemente con él.

Dado que en una composición fuerte ninguna otra parte del sistema posee enlaces hacia esos componentes internos, el recolector de basura detectará en su ciclo de revisión que también son inalcanzables y procederá a liberar su espacio sin necesidad de intervención manual.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Al diseñar una composición débil con restricciones lógicas estrictas, conocidas formalmente como invariantes de clase, resulta crucial aplicar la encapsulación de manera rigurosa. Se debe evitar por completo la exposición de las estructuras de datos subyacentes, como los arreglos primitivos, para prevenir manipulaciones no autorizadas que corrompan el frágil estado del objeto contenedor.

La clase contenedora asume la obligación de proporcionar una interfaz controlada para interactuar con sus colecciones internas. Esto implica centralizar toda la lógica en métodos específicos para añadir, eliminar o consultar elementos. Estos métodos actúan como barreras de seguridad que verifican el estricto cumplimiento de las reglas de negocio antes de aplicar cualquier modificación a la estructura.

En caso de que una solicitud externa intente vulnerar la integridad de los datos, como por ejemplo intentar eliminar a un elemento indispensable para la lógica de la clase o dejar a la estructura en un estado inconsistente, el mecanismo idóneo y estándar en la orientación a objetos es interrumpir inmediatamente el flujo del programa lanzando una excepción pertinente.

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

La adopción de estructuras dinámicas nativas proporcionadas por el lenguaje simplifica drásticamente el código necesario para la gestión interna en comparación con los arreglos primitivos. Se elimina por completo la necesidad de llevar un control manual del tamaño, la capacidad máxima y el desplazamiento tedioso de elementos al realizar eliminaciones en posiciones intermedias.

Sin embargo, el uso de estas estructuras dinámicas introduce un riesgo crítico de diseño si no se gestionan adecuadamente las respuestas de los métodos consultores. Si se devuelve directamente la referencia de la estructura interna al exterior, se produce una fuga de encapsulación grave. Cualquier clase externa podría modificar el contenido libremente, saltándose todas las comprobaciones e invariantes definidas en el contenedor.

Para resolver esta vulnerabilidad y mantener la robustez del diseño orientado a objetos, la solución teórica exige evitar el retorno de la referencia original bajo cualquier circunstancia. En su lugar, se debe devolver siempre una copia independiente de la estructura de datos o, alternativamente, proporcionar una vista de solo lectura que impida cualquier tipo de modificación destructiva desde el exterior.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva representa un patrón de diseño estructural fundamental donde una clase define, como parte de sus atributos internos, una o varias referencias directas a objetos de su misma clase. Esta técnica permite la construcción de jerarquías, anidamientos y redes de objetos del mismo tipo con una profundidad teóricamente ilimitada.

Al implementar este patrón, resulta imperativo establecer una condición de parada o caso base para evitar ciclos infinitos o errores en la lógica computacional. Esto se logra habitualmente permitiendo que la referencia recursiva pueda apuntar a un valor nulo, indicando así el final definitivo de la cadena o el origen de la jerarquía.

Este concepto teórico es la piedra angular sobre la que se construyen múltiples estructuras de datos avanzadas en la informática. Ejemplos clásicos de composiciones recursivas incluyen los nodos en las listas enlazadas, los elementos constitutivos de los árboles binarios de búsqueda, o la representación lógica de un sistema de archivos, donde un directorio puede contener a su vez otros directorios en su interior.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición bidireccionales se establecen cuando ambas partes involucradas en la relación mantienen un conocimiento explícito y mutuo sobre la otra. Es decir, el objeto contenedor posee un registro detallado de las partes que lo conforman, y simultáneamente, cada parte almacena una referencia directa e individual hacia el contenedor específico al que pertenece en ese instante.

Para implementar este nivel de entrelazamiento, se requiere definir atributos de referencia cruzada en ambas clases. El desafío técnico principal radica en mantener la coherencia absoluta y la sincronización del estado general. Cuando se vincula una parte a un contenedor, la lógica interna debe asegurar indefectiblemente que ambas referencias se actualicen al unísono.

Durante esta sincronización, es absolutamente crítico diseñar los métodos de asignación con extremo cuidado para no incurrir en bucles de llamadas infinitas. Si no se maneja correctamente, la actualización del contenedor podría disparar la actualización de la parte, y viceversa, bloqueando el programa de forma cíclica.