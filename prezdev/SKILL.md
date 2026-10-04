---
name: prezdev
description: Java clean code and coding conventions. Use when writing, reviewing, or refactoring Java code to apply naming, formatting, method design, explicit behavior, reuse, error handling, concurrency, comments, and SOLID guidelines. Requires English code, allows Spanish logs, and includes constructor injection rules for projects using Spring Boot with Lombok.
---

# Skill: Clean Code, SOLID & Java Guardrails (prezdev-rules)

Adapted with additional guardrails from [Oracle's Java Code Conventions](https://www.oracle.com/a/tech/docs/java/codeconventions.pdf).

**DIRECTIVA OBLIGATORIA PARA LA IA:**

**BAJO NINGUNA CIRCUNSTANCIA debes generar, sugerir o autocompletar CÓDIGO NUEVO que viole las reglas descritas en este documento. Toda pieza de software Java NUEVA que produzcas DEBE adherirse estrictamente a estos principios.**

**EXCEPCIÓN (CÓDIGO LEGADO):** Si la petición implica trabajar con código legado, se permite la flexibilidad necesaria para interoperar con la base existente. Sin embargo, el código nuevo añadido al sistema legado debe seguir estas reglas.

---

## REGLAS DE INTERACCIÓN Y WORKFLOW

- **Sin rutas de archivos:** No incluyas rutas de archivos ni referencias a archivos de clases en las explicaciones a menos que el usuario lo solicite explícitamente. Describe el código por componente o responsabilidad.

- **Conventional Commits:** Si se te pide crear o proponer un commit, el mensaje debe formatearse estrictamente según la especificación *Conventional Commits* (ej. `feat:`, `fix:`, `refactor:`).

---

## 1. Idioma y Nombres con Sentido (Naming)

- **Inglés obligatorio:** Nombres de clases, métodos, variables, paquetes, comentarios y cualquier texto a nivel de código DEBEN estar en inglés.

- **Excepción de Idioma (Logs):** Los mensajes de log (bitácoras) son la única excepción y pueden escribirse en español cuando sea preferible para los operadores o usuarios.

- **Revelar la intención:** El nombre debe responder por qué existe, qué hace y cómo se usa.

    - Clases/Interfaces: Sustantivos en `PascalCase`.
    - Métodos: Verbos en `lowerCamelCase`.
    - Variables: `lowerCamelCase` descriptivo. Cero letras sueltas (excepto contadores de bucle triviales).
    - Constantes: `UPPER_SNAKE_CASE`.

- **Nombres pronunciables:** Prefiere nombres fáciles de pronunciar en una conversación técnica; evita abreviaturas crípticas.

- **Nombres buscables:** Prefiere nombres específicos y fáciles de localizar mediante búsquedas en el código, evitando identificadores demasiado genéricos.

- **Sin prefijos que codifiquen el tipo:** Evita prefijos que repitan el tipo (`str`) o marquen innecesariamente campos (`m_`) e interfaces (`I`). Conserva los prefijos con significado semántico, como `is` en predicados.

- **Vocabulario consistente:** Usa el mismo término para operaciones equivalentes; evita alternar entre sinónimos sin una diferencia real de significado. Si los comportamientos son distintos, refleja esa diferencia en los nombres.

- **Contexto sin redundancia:** Añade contexto al nombre cuando ayude a entenderlo; evita repetir información que ya aporta la clase o el ámbito. No acortes nombres si con ello pierden claridad o facilidad de búsqueda.

- **Fábricas nombradas:** Si el significado de los argumentos de un constructor resulta ambiguo, considera una fábrica estática con un nombre que aclare cómo se crea el objeto. No es obligatorio sustituir constructores que ya sean claros.

## 2. Estructura de Archivos y Clases

- **Archivos y Paquetes:** Mantener un tipo público de nivel superior por archivo fuente (la clase/interfaz debe coincidir con el nombre del archivo). El tipo público debe declararse antes de los demás tipos de nivel superior del mismo archivo.

- **Separación de secciones:** Separa las secciones de un archivo fuente Java (por ejemplo, `package`, imports y declaraciones de tipos) con líneas en blanco.

- **Orden de Imports:** Los imports deben agruparse lógicamente:

    1. `java.*` / `javax.*`.
    2. Terceros (`org.*`, `com.*`).
    3. Clases del proyecto.

- **PROHIBIDO usar wildcards en imports:** Nunca usar `import paquete.*;`. Cada clase debe importarse de forma explícita.

- **NO usar FQN (Fully Qualified Names):** Usa imports en lugar de escribir nombres completos (ej. usar `List` con import, no `java.util.List`). Excepción: literales de cadena que lo requieran.

- **Orden de los miembros de la clase:**

    1. Campos/Variables estáticas.
    2. Campos/Variables de instancia.
    3. Constructores.
    4. Métodos (aplicando la regla Top-Down).

    Dentro de cada grupo de campos (estáticos y de instancia), declara primero los `public`, después los `protected` y finalmente los `private`.

## 3. Funciones y Métodos

- **Orden Top-Down (Stepdown Rule):** Si el método `A` llama al método `B`, `B` debe declararse debajo de `A`. Los puntos de entrada públicos van primero, los helpers privados después.

- **Pequeñas y de una sola cosa (SRP):** Las funciones deben hacer una sola cosa y tener un solo nivel de abstracción.

- **Pocos parámetros:** Prefiere funciones con 0–2 parámetros. Con 3 o más, revisa si el diseño puede simplificarse; no es un límite absoluto. No reduzcas el conteo ocultando datos sin relación dentro de un objeto.

- **Agrupar parámetros relacionados:** Si varios parámetros representan un mismo concepto, considera agruparlos en un objeto cohesivo en lugar de pasarlos por separado.

- **Evitar parámetros de salida:** Prefiere devolver el resultado en lugar de recibir un objeto o contenedor que el método deba rellenar. Esto no prohíbe modificar objetos cuando esa sea la responsabilidad explícita del método.

- **Evitar parámetros selectores:** Si un parámetro elige entre comportamientos distintos, especialmente un booleano, considera métodos con nombres explícitos para cada comportamiento. Esto no prohíbe booleanos que representen datos legítimos del dominio.

- **Separar consulta y modificación:** Separa las consultas de las operaciones que modifican el estado del dominio. Una consulta no debe cambiar ese estado; las modificaciones deben ser explícitas.

- **Efectos secundarios explícitos:** Evita efectos inesperados, como enviar mensajes, persistir datos o modificar otros objetos sin que el nombre y el contrato del método permitan anticiparlo. Estos efectos están permitidos cuando forman parte de su responsabilidad explícita.

- **Variables locales cerca de su uso:** Declara las variables locales cerca de su primer uso y en el ámbito más pequeño necesario. Al reorganizar declaraciones o inicializaciones, preserva el orden de ejecución y los efectos de las operaciones.

- **Predicados nombrados:** Extrae las condiciones complejas difíciles de leer a métodos booleanos con nombres que expliquen la decisión cuando eso aporte claridad. No es necesario extraer condiciones triviales ni crear envoltorios que no aporten significado.

- **Condiciones positivas:** Prefiere condiciones afirmativas cuando faciliten la lectura y evita dobles negaciones innecesarias. Mantén condiciones negativas, incluidas las guardas de casos inválidos, cuando expresen mejor el flujo.

- **Orden temporal explícito:** Cuando una operación dependa de que otra ocurra primero, expresa esa dependencia mediante parámetros, resultados o tipos en lugar de depender de un estado oculto o de recordar el orden de llamadas. No crees tipos adicionales para secuencias evidentes si no aportan claridad.

- **Encapsular cálculos de límites:** Cuando los cálculos de índices o rangos se repitan o sean confusos, centralízalos en un método u objeto con significado claro. Aclara si los límites son inclusivos o exclusivos. No es necesario crear helpers para operaciones aritméticas triviales que ya sean claras.

- **PROHIBIDO pasar llamadas a funciones como argumentos (Regla de lectura paso a paso):** Nunca llames a una función directamente dentro de la lista de argumentos de otra. Evalúa la llamada interna, guárdala en una variable local y pásala. (Ej. *Evitar* `foo(bar())`). Ejemplo correcto:

    ```java
    Result result = bar();
    foo(result);
    ```

- **Devoluciones directas (No variables desechables):** No crees variables locales solo para devolverlas en la siguiente línea.

    *Evitar:*

    ```java
    Result r = compute();
    return r;
    ```

    *Usar:*

    ```java
    return compute();
    ```

- **Retornos directos y explícitos:** Prefiere retornos booleanos directos (`return a > b;`) y expresiones condicionales simples, también para valores no booleanos, sobre cadenas de retornos `if/else` verbosas cuando mejoren la legibilidad. No es obligatorio usar ternarios si un `if/else` resulta más claro.

## 4. Formato y Estilo de Código

- **Una sentencia por línea:** No comprimas varias sentencias en una misma línea para ahorrar espacio. Cada sentencia debe ir en su propia línea.

- **Indentación y Llaves:**

    - Usar indentación de 4 espacios.
    - Las llaves de apertura van en la misma línea; las de cierre en su propia línea. Excepción: los bloques vacíos pueden mantenerse compactos cuando sean triviales.
    - **Obligatorio:** Usar siempre llaves para estructuras de control (`if`, `else`, `for`, `while`), incluso para una sola línea.

- **División de líneas largas:** Divide las líneas largas deliberadamente y evita continuaciones demasiado anidadas. Cuando una declaración o condición ocupe varias líneas, alinea las continuaciones para que el cuerpo de la sentencia siga siendo fácil de leer.

    - Si divides una llamada entre argumentos, corta después de la coma.
    - Prefiere puntos de corte en los niveles externos de una expresión y mantén juntos los grupos internos, como las expresiones entre paréntesis, cuando sea posible.

- **Espaciado visual consistente:**

    - Una línea en blanco entre métodos y para separar bloques lógicos dentro de un método.
    - Línea en blanco después de bloques de control (`if`, `for`, `while`, `switch`, `try/catch`) y antes de la siguiente instrucción, incluyendo un `return`, una llamada a un método o una declaración de variable.
    - Espacios después de palabras clave, comas y alrededor de operadores binarios.
    - Sin espacio entre el nombre de un método y `(`, tanto en declaraciones como en llamadas.

## 5. Variables y Expresiones

- **Una declaración por línea:** Mantén una declaración o variable por línea. **NO agrupar variables del mismo tipo** (Ej. *Evitar* `int level, size;`). Ejemplo correcto:

    ```java
    int level;
    int size;
    ```

- **Evitar asignaciones embebidas y múltiples:** Las asignaciones deben ser explícitas y separadas. No asignes valores dentro de otras expresiones ni encadenes asignaciones; no dependas de efectos secundarios para abreviar el código.

- **Prohibición de "Shadowing":** No declares variables locales con el mismo nombre que campos de clase o variables de niveles superiores. (Excepción: asignaciones en constructores/setters usando `this.campo = campo;`).

- **Paréntesis para aclarar la precedencia:** Usa paréntesis para hacer explícita la agrupación cuando una expresión mezcle operadores. Prioriza la claridad sobre depender de que el lector recuerde las reglas de precedencia.

- **Paréntesis en operadores ternarios:** Si la condición evalúa una expresión, debe ir entre paréntesis para mayor legibilidad (Ej. `return (x >= 0) ? x : -x;`).

- **Cero Números Mágicos:** Nunca uses valores literales directamente en el código base, **incluyendo los `case` de un `switch`**. Extráelos a constantes `static final` (excepto pequeños contadores triviales como -1, 0, 1).

- **Aritmética monetaria exacta:** Para calcular importes monetarios que requieran exactitud decimal, evita `float` y `double`. Usa `BigDecimal` o cantidades enteras en la unidad mínima correspondiente. Cuando una operación requiera redondeo, define la precisión, la escala y el criterio según la regla de negocio; no impongas dos decimales para todas las monedas.

## 6. Manejo de Errores y Control de Flujo

- **Try al inicio del método:** Cuando se requiere manejo de excepciones, el `try` debe estar al inicio del cuerpo del método. Evitar lógica previa al `try` a menos que sea inevitable.

- **Usar Excepciones en lugar de Códigos de Retorno:** Extraer el bloque del `try` si empieza a crecer.

- **Errores con contexto:** Explica qué falló y sobre qué dato u operación mediante contexto útil para diagnosticar el problema, en lugar de mensajes genéricos. No incluyas contraseñas, tokens ni datos sensibles innecesarios.

- **Conservar la causa original:** Si capturas una excepción y lanzas otra, conserva la original como causa para mantener el error y su traza; no copies únicamente su mensaje. No es necesario envolver todas las excepciones: permite que se propaguen cuando no necesites traducirlas.

- **Tipos de excepción según la respuesta al error:** Distingue los errores mediante tipos o categorías cuando quien los captura necesite actuar de forma diferente. Si varios errores requieren el mismo tratamiento, pueden compartir un tipo o categoría. No es necesario crear una clase de excepción distinta para cada detalle técnico.

- **Sin excepciones para el flujo normal:** Usa condiciones, bucles o resultados explícitos para representar situaciones válidas y normales según el contrato de la operación. No lances ni captures excepciones únicamente para dirigir ese flujo, como detectar el final de una iteración. Conserva las excepciones para los fallos; que un fallo sea previsible no lo convierte en un resultado válido.

- **Null solo con un contrato claro:** No pases ni devuelvas `null` por costumbre. Úsalo únicamente cuando tenga un significado explícito en el contrato y los consumidores sepan cómo tratarlo. Para expresar una ausencia válida, considera alternativas como `Optional` cuando aporten claridad. No es obligatorio usar `Optional` siempre ni cambiar los contratos existentes.

- **Comprobar resultados que pueden ser null:** Cuando el contrato de una API permita devolver `null`, trata esa posibilidad antes de utilizar el resultado. Distingue una ausencia válida de un fallo y responde según el contrato de la operación. No es necesario añadir comprobaciones a todas las llamadas cuyo contrato ya garantice un resultado no nulo.

- **Colecciones vacías en lugar de null:** Cuando una operación no encuentre elementos y eso sea un resultado válido, devuelve una colección vacía en lugar de `null`. Al integrar una API existente que use `null` para indicar ausencia de elementos, normaliza ese resultado cuando el contrato lo permita. No ocultes fallos devolviendo colecciones vacías y respeta el contrato de mutabilidad de la colección.

- **Comportamiento explícito del `switch`:** Todo `switch` debe incluir una rama `default`. Si un caso "cae" intencionalmente al siguiente, documentarlo con `/* falls through */`.

## 7. Objetos, Estructuras y Compartición de Código

- **Cohesión elevada:** Mantén juntos los datos y métodos que estén relacionados con la responsabilidad de la clase. Si aparecen grupos independientes de datos y operaciones, considera separarlos en clases con responsabilidades claras. No es necesario que todos los métodos utilicen todos los campos.

- **APIs mínimas:** Expón únicamente las operaciones que necesitan los consumidores de una clase y mantén privados los detalles de implementación. No hagas públicos los helpers internos sin una necesidad concreta. Al refactorizar, preserva los contratos existentes salvo que se acuerde un cambio de API.

- **Getters no automáticos:** No añadas un getter para cada campo por costumbre. Expón consultas o comportamientos que tengan sentido para los consumidores del objeto, sin revelar detalles internos innecesarios. Los getters son apropiados cuando responden a una necesidad concreta, como transportar datos mediante DTOs, ofrecer consultas necesarias o integrarse con frameworks.

- **Distinguir DTOs y objetos de dominio:** Mantén clara la diferencia entre los DTOs que transportan datos y los objetos de dominio que encapsulan comportamiento y reglas, controlando cómo cambia su estado. Evita convertir los DTOs en servicios de negocio o exponer todo el estado del objeto de dominio para que sus consumidores manipulen sus reglas. No es obligatorio usar `record` para representar DTOs.

- **Evitar acoplamientos artificiales:** No hagas que una clase dependa de otra sin relación con su responsabilidad solo para reutilizar una constante o un método. Ubica esa lógica o información en el componente al que pertenece; si representa una regla realmente compartida, colócala en un componente con esa responsabilidad. No es necesario crear una clase nueva para cada constante.

- **Clases base independientes de sus derivadas:** Evita que una clase base dependa de sus subclases concretas o compruebe sus tipos para decidir su comportamiento. Expresa las operaciones variables mediante el contrato de la clase base y permite que las subclases proporcionen sus implementaciones. Esta regla no obliga a utilizar herencia.

- **Interfaces como tipos:** Cuando solo necesites las operaciones de una interfaz, úsala como tipo de variables, parámetros y retornos en lugar de depender de una implementación concreta. Por ejemplo, declara `List` en lugar de `ArrayList` cuando baste el contrato de lista. Usa tipos concretos cuando necesites sus capacidades específicas; no es obligatorio crear una interfaz para cada clase.

- **Enums para categorías:** Cuando representes un conjunto cerrado de estados o categorías definido en el código, prefiere un `enum` en lugar de números o cadenas para expresar los valores permitidos mediante un tipo específico. No es obligatorio usar enums para categorías dinámicas que se crean en una base de datos o configuración.

- **Sin interfaces solo para constantes:** No crees una interfaz únicamente para guardar constantes ni la implementes solo para acceder a ellas. Ubica las constantes en la clase responsable o en un contenedor apropiado y accede a ellas mediante el nombre del tipo. No es necesario crear una clase separada si una constante solo pertenece a una clase; puede permanecer allí como `private static final`.

- **Separar construcción y ejecución:** Mantén la preparación de los servicios y sus conexiones separada de la lógica que realiza el trabajo. Prepara esos colaboradores en la inicialización, configuración o factorías apropiadas, de modo que las operaciones de negocio se centren en utilizarlos. Esta regla no exige una forma concreta de inyección ni prohíbe crear objetos locales sencillos, como listas u objetos de resultado, dentro de un método.

- **Configuración de alto nivel:** Define los valores ajustables, como tiempos de espera y límites de reintentos, en la configuración de la aplicación y proporciónalos a los componentes que los utilizan, en lugar de esconderlos dentro de los algoritmos. No conviertas todas las constantes en configuración: los valores fijos propios de una regla o algoritmo pueden permanecer en el código.

- **Ley de Demeter:** Evita que los consumidores de un objeto dependan de su estructura interna recorriendo cadenas de objetos para obtener datos o ejecutar operaciones. Prefiere consultas u operaciones con significado en el objeto responsable, de modo que los cambios internos no se propaguen a todos sus consumidores. No es una prohibición de todas las llamadas encadenadas: los DTOs y las APIs fluidas pueden tenerlas legítimamente.

- **Evitar sobreingeniería:** Resuelve las necesidades actuales sin añadir capas, interfaces o mecanismos genéricos solo por futuros usos hipotéticos. Introduce abstracciones cuando exista una necesidad concreta que las justifique. Esta regla no impide abstracciones útiles ni anula las reglas o la arquitectura existentes.

- **Selección mediante polimorfismo:** Cuando varios lugares repitan condiciones por tipo o categoría para elegir entre variantes de comportamiento, considera que cada variante implemente su propio comportamiento mediante un contrato común. No es obligatorio sustituir todos los `if` o `switch`; conserva las decisiones sencillas cuando introducir clases solo añada complejidad.

- **Proteger las invariantes del objeto:** Protege las condiciones que deben cumplirse siempre mediante constructores, tipos y encapsulación, en lugar de confiar en que cada consumidor las recuerde. Rechaza estados inválidos al construir el objeto y conserva sus garantías durante las modificaciones posteriores. No es obligatorio crear un tipo nuevo para cada variable; hazlo cuando aporte protección útil.

- **Extracción de Helpers de Valor:** Las conversiones/parseos repetidos (ej. `asLong`, normalizaciones) deben vivir en clases utilitarias compartidas (ej. `ValueUtils`), no duplicarse dentro de las clases de características.

- **Evitar duplicación general:** Cuando varios componentes implementen la misma regla o conocimiento, comparte esa lógica en el componente responsable. No extraigas una abstracción solo por parecido textual si los fragmentos representan reglas diferentes.

- **Evitar campos públicos mutables:** Prefiere campos privados con métodos enfocados en el comportamiento.

- **Miembros estáticos:** Acceder a través de la Clase (`Clase.metodo()`), no a través de instancias.

## 8. Comentarios

- El código debe explicarse a sí mismo. No comentes código malo, reescríbelo.

- **Comentarios actualizados:** Cuando cambie el comportamiento del código, actualiza o elimina los comentarios que hayan dejado de ser ciertos.

- **Sin código comentado:** No conserves variantes antiguas o código desactivado mediante comentarios; retíralos del código fuente. Esta regla no afecta a ejemplos explicativos de documentación.

- **Sin historial incrustado:** No mantengas registros cronológicos de cambios en comentarios del código fuente; usa el control de versiones para ese historial. Esta regla no excluye comentarios útiles que expliquen decisiones técnicas.

- **Marcadores estructurados:** Usar `TODO` (trabajo pendiente), `FIXME` (comportamiento roto) y `XXX` (código sospechoso pero funcional).

## 9. Principios SOLID

- **SRP:** Una clase/módulo debe tener solo una razón para cambiar.

- **OCP:** Abierto para extensión, cerrado para modificación.

- **LSP:** Las clases derivadas deben ser completamente sustituibles por sus clases base.

- **ISP:** Es mejor tener interfaces específicas por cliente que una general.

- **DIP:** Las políticas de alto nivel no dependen de detalles de bajo nivel; ambos dependen de abstracciones.

## 10. Ecosistema Spring Boot y Lombok

- **Inyección de Dependencias (Constructor vs Autowired):** En cualquier **código nuevo** dentro de un proyecto que use Spring Boot y Lombok, la inyección de dependencias DEBE realizarse a través del constructor utilizando las anotaciones de Lombok (como `@RequiredArgsConstructor` junto con campos `private final`).

- **Cero `@Autowired` en código nuevo:** Está estrictamente prohibido usar la anotación `@Autowired` para inyectar dependencias en clases creadas desde cero.

## 11. Concurrencia

Aplica estas reglas cuando el código ejecute tareas concurrentes o acceda a recursos compartidos entre hilos.

No introduzcas concurrencia si la operación no la necesita.

- **Separar concurrencia y lógica de negocio:**

    - Separa la coordinación de tareas y la administración de hilos de las operaciones de negocio,
      para poder probar y cambiar cada responsabilidad de forma independiente.
    - Esta separación no garantiza por sí sola que los servicios sean seguros para ejecutarse simultáneamente;
      protege también sus datos y recursos compartidos.

- **Limitar el estado compartido:**

    - Reduce y encapsula los datos mutables compartidos entre hilos.
    - Define quién puede acceder a ellos y utiliza un mecanismo coherente que garantice la visibilidad
      de los cambios y la protección de sus invariantes.
    - Un campo privado o una referencia `final` no convierten automáticamente su contenido mutable
      en seguro para concurrencia.

- **Copias para aislar datos:**

    - Cuando aporte claridad y sea compatible con el contrato, considera datos inmutables o copias
      independientes para cada tarea en lugar de compartir objetos mutables.
    - Obtén las copias de forma segura respecto a las modificaciones concurrentes.
    - Una copia superficial de una colección sigue compartiendo sus elementos; evita que esos elementos
      mutables mantengan un estado compartido sin protección.

- **Tareas independientes:**

    - Prefiere tareas con entradas y estado de trabajo propios, y combina sus resultados mediante
      un mecanismo seguro cuando sea necesario.
    - Una variable local que referencia un objeto compartido no aísla ese objeto.
    - Conserva las dependencias y el orden que exijan las reglas de negocio; no paralelices operaciones
      dependientes solo para aumentar el número de tareas.

- **Bibliotecas concurrentes estándar:**

    - Prefiere las herramientas del JDK, como `ExecutorService`, `BlockingQueue`, colecciones concurrentes
      y clases atómicas, frente a implementar mecanismos equivalentes manualmente.
    - Selecciona cada herramienta según su contrato y la versión de Java del proyecto.
    - No es necesario sustituir las colecciones ordinarias cuando sus datos están confinados a una tarea
      o correctamente protegidos.

- **Operaciones compuestas atómicas:**

    - Cuando una secuencia de lectura, comprobación y modificación deba ser indivisible, protege toda
      la operación mediante sincronización o una API que garantice la atomicidad requerida.
    - Varias llamadas individualmente seguras no hacen atómica su combinación.
    - `volatile` no hace atómicas operaciones compuestas, y las clases atómicas de una sola variable
      no protegen por sí solas invariantes entre varios datos.

- **Secciones críticas mínimas:**

    - Mantén bajo bloqueo únicamente el trabajo necesario para proteger el estado compartido y sus invariantes.
    - Evita operaciones lentas, E/S o llamadas a código externo dentro de la sección crítica cuando puedan
      realizarse fuera sin comprometer la corrección.
    - No fragmentes una operación que necesita ser atómica; si utilizas bloqueos explícitos,
      garantiza su liberación incluso ante excepciones.

- **Orden consistente de bloqueos:**

    - Cuando una operación necesite varios bloqueos, define y respeta un orden de adquisición común
      en todos los caminos que puedan acceder a esos recursos.
    - Evita dependencias circulares y esperas por tareas que necesiten un bloqueo que mantienes adquirido.
    - Revisa también los bloqueos internos de los componentes llamados; ordenar los bloqueos visibles
      no garantiza por sí solo la ausencia de interbloqueos.

- **Cierre planificado y probado:**

    - Define quién administra el ciclo de vida de los ejecutores y qué sucede con las tareas pendientes
      y activas al cerrar: completarlas o cancelarlas según el contrato.
    - Coordina las señales de cierre, las esperas y la liberación de recursos, con límites de espera apropiados.
    - Respeta las interrupciones y comprueba el cierre con tareas en ejecución o bloqueadas;
      solicitar una cancelación no garantiza su terminación inmediata.
    - No cierres ejecutores compartidos cuyo ciclo de vida pertenece a otro componente.

- **Lógica secuencial probada:**

    - Prueba primero las operaciones de negocio de forma secuencial para distinguir sus errores
      de los problemas de coordinación entre hilos.
    - Después comprueba su integración concurrente.
    - Superar las pruebas secuenciales no demuestra que el uso simultáneo sea seguro
      ni sustituye las pruebas de concurrencia.

- **Pruebas de estrés multiplataforma:**

    - Para cambios relevantes en código concurrente, prueba con distintas cargas, cantidades de tareas
      e hilos y, cuando sea viable, con más hilos ejecutables que núcleos disponibles.
    - Incluye las plataformas y configuraciones de ejecución de destino disponibles, en proporción al riesgo.
    - Superar estas pruebas reduce el riesgo, pero no demuestra la ausencia de condiciones de carrera
      o interbloqueos.

- **Investigar fallos intermitentes:**

    - No descartes un fallo solo porque desaparezca al repetir la ejecución.
    - Investiga posibles carreras, problemas de visibilidad, interbloqueos u otras causas,
      conservando los datos necesarios para reproducirlo.
    - Cuando identifiques la causa, añade una prueba de regresión apropiada;
      no atribuyas automáticamente todo fallo intermitente a la concurrencia.

- **Configuración de concurrencia ajustable:**

    - Aplica la regla de configuración de alto nivel a parámetros como la cantidad de trabajadores,
      la capacidad de las colas y los tiempos de espera.
    - Permite probar configuraciones válidas sin modificar los algoritmos y considera los límites
      de los recursos utilizados.
    - No exige ajustes automáticos ni cambios en caliente si la aplicación no los necesita.

- **Intercalados controlados en pruebas:**

    - Diseña pruebas que permitan reproducir órdenes de ejecución relevantes mediante barreras,
      señales o mecanismos de planificación controlada.
    - Comprueba las invariantes y los resultados, no solo que las tareas terminen.
    - No dependas únicamente de pausas arbitrarias para coordinar una prueba ni añadas retrasos a producción
      para ocultar carreras; las variaciones de temporización pueden complementar las pruebas de estrés,
      pero no garantizan explorar todos los intercalados.

Referencias: [Código limpio, capítulo sobre concurrencia](https://elhacker.info/manuales/Lenguajes%20de%20Programacion/Codigo%20limpio%20-%20Robert%20Cecil%20Martin.pdf)
y [documentación de concurrencia del JDK](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/package-summary.html).

Utiliza la documentación correspondiente a la versión de Java del proyecto.
