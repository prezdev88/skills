---
name: prezdev
description: Java clean code and coding conventions. Use when writing, reviewing, or refactoring Java code to apply naming, formatting, method design, explicit behavior, reuse, error handling, comments, and SOLID guidelines. Requires English code, allows Spanish logs, and includes constructor injection rules for projects using Spring Boot with Lombok.
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

## 6. Manejo de Errores y Control de Flujo

- **Try al inicio del método:** Cuando se requiere manejo de excepciones, el `try` debe estar al inicio del cuerpo del método. Evitar lógica previa al `try` a menos que sea inevitable.

- **Usar Excepciones en lugar de Códigos de Retorno:** Extraer el bloque del `try` si empieza a crecer.

- **Comportamiento explícito del `switch`:** Todo `switch` debe incluir una rama `default`. Si un caso "cae" intencionalmente al siguiente, documentarlo con `/* falls through */`.

## 7. Objetos, Estructuras y Compartición de Código

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
