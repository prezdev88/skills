---
name: prezdev
description: Java code style guardrails for clean code refactors. Use when writing or reviewing Java code to enforce imports over FQN, top-down method order, shared utility extraction, and English code style, while allowing logs in Spanish.
---

# Skill: Clean Code, Clean Architecture & Java Guardrails (prezdev-rules)

Adapted with additional guardrails from [Oracle's Java Code Conventions](https://www.oracle.com/a/tech/docs/java/codeconventions.pdf).

**DIRECTIVA OBLIGATORIA PARA LA IA:**

**BAJO NINGUNA CIRCUNSTANCIA debes generar, sugerir o autocompletar CÓDIGO NUEVO que viole las reglas descritas en este documento. Toda pieza de software Java NUEVA que produzcas DEBE adherirse estrictamente a estos principios.**

**EXCEPCIÓN (CÓDIGO LEGADO):** Si la petición implica trabajar con código legado, se permite la flexibilidad necesaria para interoperar con la base existente. Sin embargo, el código nuevo añadido al sistema legado debe seguir estas reglas.

---

## REGLAS DE INTERACCIÓN Y WORKFLOW

* **Sin rutas de archivos:** No incluyas rutas de archivos ni referencias a archivos de clases en las explicaciones a menos que el usuario lo solicite explícitamente. Describe el código por componente o responsabilidad.
* **Conventional Commits:** Si se te pide crear o proponer un commit, el mensaje debe formatearse estrictamente según la especificación *Conventional Commits* (ej. `feat:`, `fix:`, `refactor:`).

---

## 1. Idioma y Nombres con Sentido (Naming)

* **Inglés obligatorio:** Nombres de clases, métodos, variables, paquetes, comentarios y cualquier texto a nivel de código DEBEN estar en inglés.
* **Excepción de Idioma (Logs):** Los mensajes de log (bitácoras) son la única excepción y pueden escribirse en español cuando sea preferible para los operadores o usuarios.
* **Revelar la intención:** El nombre debe responder por qué existe, qué hace y cómo se usa.

  * Clases/Interfaces: Sustantivos en `PascalCase`.
  * Métodos: Verbos en `lowerCamelCase`.
  * Variables: `lowerCamelCase` descriptivo. Cero letras sueltas (excepto contadores de bucle triviales).
  * Constantes: `UPPER_SNAKE_CASE`.

## 2. Estructura de Archivos y Clases

* **Archivos y Paquetes:** Mantener un tipo público de nivel superior por archivo fuente (la clase/interfaz debe coincidir con el nombre del archivo). El tipo público debe declararse antes de los demás tipos de nivel superior del mismo archivo.
* **Orden de Imports:** Los imports deben agruparse lógicamente:

  1. `java.*` / `javax.*`.
  2. Terceros (`org.*`, `com.*`).
  3. Clases del proyecto.

* **PROHIBIDO usar wildcards en imports:** Nunca usar `import paquete.*;`. Cada clase debe importarse de forma explícita.
* **NO usar FQN (Fully Qualified Names):** Usa imports en lugar de escribir nombres completos (ej. usar `List` con import, no `java.util.List`). Excepción: literales de cadena que lo requieran.
* **Orden de los miembros de la clase:**

  1. Campos/Variables estáticas.
  2. Campos/Variables de instancia.
  3. Constructores.
  4. Métodos (aplicando la regla Top-Down).

## 3. Funciones y Métodos

* **Orden Top-Down (Stepdown Rule):** Si el método `A` llama al método `B`, `B` debe declararse debajo de `A`. Los puntos de entrada públicos van primero, los helpers privados después.
* **Pequeñas y de una sola cosa (SRP):** Las funciones deben hacer una sola cosa y tener un solo nivel de abstracción.
* **PROHIBIDO pasar llamadas a funciones como argumentos (Regla de lectura paso a paso):** Nunca llames a una función directamente dentro de la lista de argumentos de otra. Evalúa la llamada interna, guárdala en una variable local y pásala. (Ej. *Evitar* `foo(bar())`). Ejemplo correcto:

  ```java
  Result result = bar();
  foo(result);
  ```

* **Devoluciones directas (No variables desechables):** No crees variables locales solo para devolverlas en la siguiente línea.

  *Evitar:*

  ```java
  Result r = compute();
  return r;
  ```

  *Usar:*

  ```java
  return compute();
  ```

* **Retornos directos y explícitos:** Prefiere retornos booleanos directos (`return a > b;`) y expresiones condicionales simples, también para valores no booleanos, sobre cadenas de retornos `if/else` verbosas cuando mejoren la legibilidad. No es obligatorio usar ternarios si un `if/else` resulta más claro.

## 4. Formato y Estilo de Código

* **Una sentencia por línea:** No comprimas varias sentencias en una misma línea para ahorrar espacio. Cada sentencia debe ir en su propia línea.
* **Indentación y Llaves:**

  * Usar indentación de 4 espacios.
  * Las llaves de apertura van en la misma línea; las de cierre en su propia línea. Excepción: los bloques vacíos pueden mantenerse compactos cuando sean triviales.
  * **Obligatorio:** Usar siempre llaves para estructuras de control (`if`, `else`, `for`, `while`), incluso para una sola línea.

* **División de líneas largas:** Divide las líneas largas deliberadamente y evita continuaciones demasiado anidadas. Cuando una declaración o condición ocupe varias líneas, alinea las continuaciones para que el cuerpo de la sentencia siga siendo fácil de leer.
* **Espaciado visual consistente:**

  * Una línea en blanco entre métodos y para separar bloques lógicos dentro de un método.
  * Línea en blanco después de bloques de control (`if`, `for`, `while`, `switch`, `try/catch`) y antes de la siguiente instrucción, incluyendo un `return`, una llamada a un método o una declaración de variable.
  * Espacios después de palabras clave, comas y alrededor de operadores binarios.
  * Sin espacio entre el nombre de un método y `(`, tanto en declaraciones como en llamadas.

## 5. Variables y Expresiones

* **Una declaración por línea:** Mantén una declaración o variable por línea. **NO agrupar variables del mismo tipo** (Ej. *Evitar* `int level, size;`). Ejemplo correcto:

  ```java
  int level;
  int size;
  ```

* **Evitar asignaciones embebidas y múltiples:** Las asignaciones deben ser explícitas y separadas. No asignes valores dentro de otras expresiones ni encadenes asignaciones; no dependas de efectos secundarios para abreviar el código.
* **Prohibición de "Shadowing":** No declares variables locales con el mismo nombre que campos de clase o variables de niveles superiores. (Excepción: asignaciones en constructores/setters usando `this.campo = campo;`).
* **Paréntesis para aclarar la precedencia:** Usa paréntesis para hacer explícita la agrupación cuando una expresión mezcle operadores. Prioriza la claridad sobre depender de que el lector recuerde las reglas de precedencia.
* **Paréntesis en operadores ternarios:** Si la condición evalúa una expresión, debe ir entre paréntesis para mayor legibilidad (Ej. `return (x >= 0) ? x : -x;`).
* **Cero Números Mágicos:** Nunca uses valores literales directamente en el código base, **incluyendo los `case` de un `switch`**. Extráelos a constantes `static final` (excepto pequeños contadores triviales como -1, 0, 1).

## 6. Manejo de Errores y Control de Flujo

* **Try al inicio del método:** Cuando se requiere manejo de excepciones, el `try` debe estar al inicio del cuerpo del método. Evitar lógica previa al `try` a menos que sea inevitable.
* **Usar Excepciones en lugar de Códigos de Retorno:** Extraer el bloque del `try` si empieza a crecer.
* **Comportamiento explícito del `switch`:** Todo `switch` debe incluir una rama `default`. Si un caso "cae" intencionalmente al siguiente, documentarlo con `/* falls through */`.

## 7. Objetos, Estructuras y Compartición de Código

* **Extracción de Helpers de Valor:** Las conversiones/parseos repetidos (ej. `asLong`, normalizaciones) deben vivir en clases utilitarias compartidas (ej. `ValueUtils`), no duplicarse dentro de las clases de características.
* **Evitar campos públicos mutables:** Prefiere campos privados con métodos enfocados en el comportamiento.
* **Miembros estáticos:** Acceder a través de la Clase (`Clase.metodo()`), no a través de instancias.

## 8. Comentarios

* El código debe explicarse a sí mismo. No comentes código malo, reescríbelo.
* **Marcadores estructurados:** Usar `TODO` (trabajo pendiente), `FIXME` (comportamiento roto) y `XXX` (código sospechoso pero funcional).

## 9. Principios SOLID

* **SRP:** Una clase/módulo debe tener solo una razón para cambiar.
* **OCP:** Abierto para extensión, cerrado para modificación.
* **LSP:** Las clases derivadas deben ser completamente sustituibles por sus clases base.
* **ISP:** Es mejor tener interfaces específicas por cliente que una general.
* **DIP:** Las políticas de alto nivel no dependen de detalles de bajo nivel; ambos dependen de abstracciones.

## 10. Ecosistema Spring Boot y Lombok

* **Inyección de Dependencias (Constructor vs Autowired):** En cualquier **código nuevo** dentro de un proyecto que use Spring Boot y Lombok, la inyección de dependencias DEBE realizarse a través del constructor utilizando las anotaciones de Lombok (como `@RequiredArgsConstructor` junto con campos `private final`).
* **Cero `@Autowired` en código nuevo:** Está estrictamente prohibido usar la anotación `@Autowired` para inyectar dependencias en clases creadas desde cero.
