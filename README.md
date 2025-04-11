
# Diseño de Eslabones con Simulink y Simscape Multibody

## 1) Diseño de eslabones

El diseño de eslabones en entornos de simulación como **Simulink**, utilizando el complemento **Simscape Multibody**, constituye una herramienta clave para representar y analizar el comportamiento de sistemas mecánicos complejos. En este contexto, los *eslabones* son elementos rígidos que transmiten fuerzas o movimientos dentro de un mecanismo. Se utilizan en una amplia variedad de aplicaciones, desde brazos robóticos hasta sistemas de suspensión automotriz o mecanismos de transmisión como la biela-manivela.

Dentro del entorno de Simulink, los eslabones se modelan mediante el bloque `Solid`, el cual permite especificar parámetros físicos fundamentales como:

- Masa
- Volumen
- Centro de gravedad
- Geometría

La conectividad entre los cuerpos rígidos se logra mediante **juntas mecánicas** (*joints*), que definen la relación espacial entre ellos y los grados de libertad del sistema. Algunas de las juntas más comunes incluyen:

- 🔧 **Revolute Joint**: permite la rotación relativa entre dos sólidos alrededor de un eje fijo. Es comparable a una bisagra o a una articulación como el codo en un brazo robótico.


![image](https://github.com/user-attachments/assets/e701a845-b586-4226-b350-0fbb9ef036fe)

***Fig 1. Revolute Joing***


- 🔧 **Prismatic Joint**: restringe el movimiento a una sola dimensión lineal. Es similar al comportamiento de un pistón o actuador lineal.

![image](https://github.com/user-attachments/assets/271a360b-e82a-4d06-ae96-bbde94dd4e32)

***Fig 2. Prismatic Joing***


Además, se pueden integrar **actuadores** y **sensores** en el modelo para aplicar fuerzas, torques, o movimientos predefinidos, y para medir variables físicas como:

- Ángulos de rotación
- Velocidades angulares
- Posiciones lineales

Una herramienta fundamental en la interacción entre el entorno físico y el matemático de Simulink es el bloque `PS Converter`, que permite transformar señales convencionales (como senoidales o impulsos) en señales físicas aceptadas por Simscape. Esto facilita, por ejemplo:

- Prescribir entradas dinámicas a las juntas
- Aplicar torques variables
- Controlar movimientos con señales generadas por algoritmos o controladores

![image](https://github.com/user-attachments/assets/80382fda-dda3-4192-943d-244b9882a5ac)

***Fig 3. PS Converter***

---

## 2) Ejemplo

💡***Ejemplo 1***
A continuación, se presentará un ejemplo de simulación mecánica que ilustra el comportamiento dinámico de un cubo en movimiento. Este modelo tiene como objetivo aplicar los conceptos y componentes previamente descritos, tales como eslabones rígidos, juntas y señales físicas dentro del entorno de Simulink con Simscape Multibody.

En particular, el sistema simulado representa un movimiento oscilatorio vertical del cubo, caracterizado por un desplazamiento alternante de subida y bajada a lo largo de un único eje (movimiento traslacional unidimensional). Este comportamiento se implementa mediante una Prismatic Joint, la cual restringe el movimiento del cuerpo rígido a un solo grado de libertad lineal.

La trayectoria oscilatoria del cubo se prescribe a través de una señal física senoidal, convertida desde el entorno de Simulink mediante un bloque PS Converter, lo que permite generar un movimiento de ida y vuelta continuo con frecuencia y amplitud definidas.

A partir del esquema presentado a continuación, se procede a realizar una descripción concisa del sistema, destacando sus componentes principales y su funcionamiento dentro del entorno de simulación.

![image](https://github.com/user-attachments/assets/0c2f4eae-404a-4449-b724-8683042abba2)

***Fig 4. Ejemplo***

 El esquema mostrado a continuación representa una configuración básica en **Simscape Multibody** para la simulación de un sistema mecánico simple. En él se emplean diversos bloques fundamentales que permiten ilustrar el movimiento oscilatorio vertical de un cuerpo rígido. A continuación, se describen los componentes principales utilizados:

- **Solver Configuration**, **World Frame** y **Mechanism Configuration**:  
  Estos tres bloques constituyen la base estructural mínima necesaria para inicializar y ejecutar correctamente una simulación física en Simscape Multibody. Permiten definir el entorno de referencia, los parámetros del solucionador y la configuración general del mecanismo.

- **Sine Wave**:  
  Genera una señal senoidal que define la trayectoria oscilatoria deseada, representando el desplazamiento periódico que será aplicado al sistema.

- **Simulink-PS Converter**:  
  Transforma la señal senoidal generada en Simulink en una señal física compatible con Simscape, permitiendo su aplicación directa sobre componentes del modelo físico.

- **Prismatic Joint**:  
  Define un grado de libertad lineal entre dos cuerpos, restringiendo el movimiento del cubo a una traslación uniaxial (de arriba hacia abajo) a lo largo de un eje definido.

- **Brick Solid**:  
  Representa el cuerpo rígido (el cubo) en la simulación. Se definen sus propiedades físicas como forma, dimensiones y masa.


### Configuracion principal mechanism Configuration

El bloque **Mechanism Configuration** tiene como propósito establecer los parámetros globales del entorno de simulación mecánica. En este caso específico, se utiliza para habilitar la acción de la **gravedad**, la cual se ha definido actuando a lo largo del eje **Y**. Esta configuración asegura que las fuerzas gravitacionales influyan correctamente sobre el movimiento del cuerpo rígido (el cubo) durante la simulación.

Como se puede observar en la **Figura 5**, la dirección de la gravedad ha sido orientada negativamente sobre el eje Y, lo que simula un entorno físico realista en el que el cubo tiende a desplazarse hacia abajo cuando no se aplican fuerzas externas compensatorias.


![image](https://github.com/user-attachments/assets/14da1f3e-f8b1-4237-8312-4ab7c63b3b5f)

***Fig 5. mechanism Configuration***

### Configuracion Sine Wane

La **Figura 6** muestra los parámetros establecidos para el bloque **Sine Wave**, el cual se encarga de generar una señal senoidal que actúa como entrada dinámica para el sistema.

Los valores configurados son los siguientes:

- **Amplitude:** `0.5`  
  Define la amplitud máxima de la señal, es decir, el desplazamiento máximo que el cubo alcanzará en su movimiento oscilatorio vertical.

- **Bias:** `0`  
  No se aplica ningún desplazamiento constante a la señal, lo que indica que oscila simétricamente respecto al cero.

- **Samples per period:** `10`  
  La señal se discretiza en 10 muestras por ciclo completo, lo cual influye directamente en la resolución temporal de la onda.

- **Number of offset samples:** `0`  
  No se añade ningún desfase temporal a la señal.

- **Sample time:** `0`  
  Este valor indica que se utilizará el tiempo de simulación por defecto, es decir, el bloque generará una señal continua basada en el tiempo del solver.

- **Sine type:** `Sample based`  
  Se emplea una onda senoidal basada en muestreo, ideal para simulaciones que requieren un control preciso sobre el número de muestras por período.

- **Interpret vector parameters as 1-D:** Activado  
  Garantiza que los parámetros definidos como vectores se interpreten como vectores unidimensionales (1-D), lo cual es adecuado para señales escalares.

Esta configuración permite generar una señal senoidal suave y periódica, adecuada para simular un movimiento oscilatorio controlado del cubo a lo largo del eje Y mediante una **Prismatic Joint**.

![image](https://github.com/user-attachments/assets/1f4c2434-c4f9-4774-b41b-acf0590ff6ce)

***Fig 6. Sine Wane***

### Configuracion Prismatic Joing

La **Figura 7** muestra la configuración del bloque **Prismatic Joint**, empleado para restringir el movimiento relativo entre dos cuerpos rígidos a lo largo de un solo eje lineal. En este caso, se ha configurado la **primitiva prismatic Z (Pz)**, lo que indica que el movimiento permitido se realiza únicamente sobre el eje **Z** del sistema de coordenadas.

**Parámetros configurados:***

- **Actuation**:
  - **Force:** `Automatically Computed`  
    El bloque calcula automáticamente la fuerza requerida para que el cuerpo se mueva según la señal de entrada prescrita, considerando las fuerzas externas, como la gravedad.
  
  - **Motion:** `Provided by Input`  
    El desplazamiento del cuerpo móvil no se determina por las leyes físicas del sistema, sino que es impuesto directamente desde una entrada externa. Esta entrada proviene de una señal física (por ejemplo, una onda senoidal convertida desde Simulink mediante un **Simulink-PS Converter**).

- **Sensing**:
  - ✅ **Position:** Activada  
    Esta opción permite medir en tiempo real la posición del cuerpo a lo largo del eje Z. La activación de esta opción es útil para monitoreo, análisis posterior o para la implementación de lazos de control en tiempo real.
  
  - Las demás opciones (Velocity, Acceleration, Actuator Force, etc.) no están activadas en esta configuración, pero podrían habilitarse si se requiere una observación más detallada del comportamiento dinámico del sistema.

Esta configuración, junto con una entrada senoidal, genera un **movimiento oscilatorio vertical prescrito** del cuerpo (cubo) en el eje Z. Gracias al sensor de posición activado, es posible visualizar y registrar la trayectoria del cubo a lo largo del tiempo, lo que resulta útil para validar el comportamiento deseado o comparar contra otras estrategias de control.

![image](https://github.com/user-attachments/assets/530f6d3e-e831-4a27-9c1a-34991b193078)

***Fig 7. Prismatic Joing***

### Configuracion Brick Solid

La **Figura 7** muestra la confirguración para la creación de un cubo.

Se ha especificado un vector de dimensiones de la forma `[1 1 1]`, lo que significa que el cuerpo tiene:

  - **Ancho (X):** 1 metro  
  - **Alto (Y):** 1 metro  
  - **Profundidad (Z):** 1 metro

  Esto configura un cubo perfecto de **1 m³** en el entorno de simulación.

- **Unidad:**  
  Las unidades están definidas en metros (`m`), lo cual es coherente con la mayoría de los entornos físicos simulados y facilita el análisis dimensional y de escalado.

- **Compile-time:**  
  Esta opción indica que las dimensiones están definidas en tiempo de compilación, es decir, no cambian durante la simulación.

![image](https://github.com/user-attachments/assets/15d44cf6-9d75-4eac-bdeb-5ad4b7a29ddd)

***Fig 8. Brick Solid***

### SIMULACION

Durante la ejecución de la simulación, se puede observar el comportamiento dinámico del cubo creado mediante el bloque **Solid**. Este cubo está acoplado a una **junta prismática (Prismatic Joint)**, la cual permite desplazamientos exclusivamente a lo largo de un eje lineal, en este caso, el eje vertical (eje Y).

Gracias a la señal senoidal aplicada como entrada de movimiento al **Prismatic Joint**, el cubo ejecuta un **movimiento oscilatorio de traslación vertical**, subiendo y bajando de forma periódica a lo largo del tiempo.

En la **Figura 9** se puede ver el cubo durante uno de los ciclos de su movimiento oscilante. El cubo aparece desplazado hacia una posición inferior, lo cual confirma la acción de la onda senoidal que modula su posición al igual que en la **Figura 10** ya que el cubo aparece desplazado hacia una posición inferior.

En la animación de la simulación se aprecia claramente cómo el cubo:

- Asciende y desciende suavemente en función de la amplitud y frecuencia de la onda senoidal configurada.
- Mantiene una trayectoria rectilínea sobre el eje **Y**, gracias a las restricciones impuestas por el **Prismatic Joint**.
- Responde con fluidez al perfil de entrada definido, evidenciando un correcto acoplamiento entre el mundo físico (Simscape) y el entorno de señales matemáticas (Simulink).

Este resultado valida el modelo construido, confirmando que la interacción entre los bloques ha sido correctamente configurada para simular un comportamiento físico realista.

![image](https://github.com/user-attachments/assets/ca005403-439f-4eb2-86da-5df6b42d9c85)

***Fig 9. Movimiento del cubo inferior***

![image](https://github.com/user-attachments/assets/60dd17f3-9c65-4ed6-9a59-77b6c9839e58)

***Fig 10. Movimiento del cubo superior***

## 3) Ejercicios

### ✅ Caso 1: Sistema biela-manivela-corredera

Este mecanismo clásico puede modelarse con tres cuerpos rígidos: manivela, biela y pistón. 

![image](https://github.com/user-attachments/assets/570e2076-b81d-430f-9586-223987135b0b)

***Fig 11. Esquema del Sistema Biela-Manivela-Corredera***

La **Figura 11** representa la implementación en Simulink/Simscape Multibody de un mecanismo clásico compuesto por **biela**, **manivela** y **corredera**. Este tipo de sistema es ampliamente utilizado en aplicaciones como motores de combustión interna y compresores, debido a su capacidad de convertir un **movimiento rotacional** en **movimiento lineal**.

#### Descripción del Modelo

El modelo se estructura a partir de varios bloques fundamentales de Simscape Multibody:

- **Solver Configuration**: Configura el solucionador para realizar la simulación física.
- **World Frame**: Define el sistema de referencia inercial global.
- **Mechanism Configuration**: Permite ajustar condiciones globales del sistema, como la gravedad.
- **Simulink-PS Converter**: Convierte una señal senoidal de entrada de Simulink en una señal física (PS) utilizable por los bloques de Simscape.
- **PS-Simulink Converter**: Convierte señales físicas de salida nuevamente al dominio de Simulink para su visualización (por ejemplo, en el Scope).

#### Componentes del Mecanismo

1. **Manivela**: 
   - Bloque sólido que gira continuamente alrededor de un eje fijo mediante una **Revolute Joint** (articulación rotacional).
   - El movimiento rotacional se genera mediante la señal senoidal conectada a la junta rotacional, simulando un eje impulsor.

2. **Biela**: 
   - Conectada entre la manivela y la corredera mediante dos juntas **Revolute Joint**.
   - Transmite el movimiento rotacional de la manivela a la corredera en forma de desplazamiento lineal.

3. **Corredera**: 
   - Representada como un sólido que se mueve linealmente gracias a una **Prismatic Joint**.
   - Esta junta limita el movimiento a lo largo de un solo eje (en este caso, lineal horizontal o vertical, según la configuración del modelo).
   - El desplazamiento de la corredera se visualiza mediante un bloque **Scope**.

4. **Rigid Transform**:
   - Se utiliza para establecer posiciones relativas entre los distintos componentes sólidos y ajustar alineaciones físicas.

#### Funcionalidad

El sistema convierte el **movimiento oscilatorio rotacional de la manivela**, provocado por la onda senoidal, en un **movimiento alternativo rectilíneo de la corredera**. La biela actúa como el eslabón intermedio que transmite y adapta el movimiento entre las otras dos partes.

Este modelo permite observar en tiempo real cómo se comporta un sistema mecánico real bajo condiciones de entrada dinámicas y es útil tanto en simulaciones de diseño como en estudios de control y dinámica de mecanismos.

#### Simulación

En Las **Figura 12** y **Figura 13**  se muestran capturas de la simulación en Simscape Multibody del sistema biela-manivela-corredera. En esta vista, se pueden observar claramente los tres elementos principales del mecanismo:

- **Manivela** (bloque azul a la izquierda),
- **Biela** (elemento en color magenta),
- **Corredera** (bloque marrón a la derecha).

![image](https://github.com/user-attachments/assets/3d3dbf0d-e8b4-4666-889d-926d9f08844f)
 
***Fig 12. simulacion1 del Sistema Biela-Manivela-Corredera***

![image](https://github.com/user-attachments/assets/a766ed80-13c7-47f0-99fa-4f7484e5b91f)

***Fig 13. simulacion2 del Sistema Biela-Manivela-Corredera***

En la animación se puede notar que:

- El conjunto de cuerpos rígidos reacciona de manera continua a la entrada de onda senoidal.
- La **biela** actúa como eslabón intermediario dinámico, cambiando de orientación para acompañar tanto el giro de la manivela como el desplazamiento de la corredera.
- El sistema completo funciona de manera armónica, simulando fielmente un **ciclo mecánico completo**, como el que ocurre en el pistón de un motor de combustión interna.

### ✅ Caso 2: tres eslabones

El modelo de un mecanismo compuesto por **tres eslabones** conectados mediante **juntas rotacionales (Revolute Joints)** permite representar con fidelidad el comportamiento de sistemas articulados como brazos robóticos, extremidades mecánicas o manipuladores articulados.

La **Figura 14** representa un sistema mecánico compuesto por **tres eslabones** conectados secuencialmente mediante **juntas rotacionales (Revolute Joints)**. Este tipo de configuración es comúnmente utilizada en mecanismos articulados como brazos robóticos y manipuladores.

![image](https://github.com/user-attachments/assets/0a2a3750-5ff9-4512-9b81-6f2b4f8d5496)

***Fig 14. Esquema de diseño de 3 eslabones***

### Componentes del Modelo

- **Solver Configuration**: Define los parámetros de resolución del modelo físico.
- **World Frame**: Establece el marco de referencia global para la simulación.
- **Mechanism Configuration**: Permite especificar condiciones físicas del entorno como la gravedad.
- **Rigid Transform**: Posiciona el primer eslabón respecto al marco global.


### Estructura de los Eslabones

1. **Eslabón 1**
   - Conectado al **World Frame** mediante una **Revolute Joint**.
   - Permite la rotación inicial del sistema.
   - Simula una base giratoria.

2. **Eslabón 2**
   - Unido al extremo del eslabón 1 mediante una segunda **Revolute Joint**.
   - Esta articulación permite el movimiento relativo entre el primer y segundo segmento.
   - El bloque de sólido representa su geometría y masa.

3. **Eslabón 3**
   - Conectado al final del segundo eslabón a través de una tercera **Revolute Joint**.
   - Funciona como el extremo libre o efector del sistema.

### Funcionalidad del Sistema

- **Actuación rotacional**: A través de las juntas rotacionales, se puede aplicar movimiento controlado a cada eslabón.
- **Medición angular**: Mediante sensores en las juntas se puede obtener la posición angular de cada articulación.
- **Salida a Scope**: El ángulo de alguna de las juntas es enviado a un bloque `Scope` mediante un `PS-Simulink Converter`, permitiendo visualizar la respuesta dinámica del sistema.

### Simulación.

Las **Figuras 15, 16 y 17** corresponden a diferentes momentos capturados durante la simulación del mecanismo de **tres eslabones** articulados mediante juntas rotacionales (`Revolute Joint`). Este sistema puede asociarse conceptualmente con un brazo robótico en movimiento, en el cual cada eslabón se mueve como resultado de las señales de entrada que provocan rotaciones en las articulaciones.

![image](https://github.com/user-attachments/assets/7fbfee20-4f56-4bc3-953c-7f3de4c63acc)

***Fig 15. Simulacion 1 de diseño de 3 eslabones***

![image](https://github.com/user-attachments/assets/657b20b4-5b67-417d-82ad-c2e449923a1d)

***Fig 16. Simulacion 2 de diseño de 3 eslabones***

![image](https://github.com/user-attachments/assets/1091a68a-ea4d-439d-9b28-08f911012cfa)

***Fig 17. Simulacion 3 de diseño de 3 eslabones***


### ✅ Caso 3: Suspensión automotriz

En un sistema de suspensión:

- Las ruedas están conectadas al chasis por eslabones.
- Se usan juntas rotacionales y traslacionales para simular los grados de libertad del sistema.
- Se pueden incluir resortes y amortiguadores para representar las fuerzas de suspensión.

En la **Figura 18** se muestra un modelo básico en Simscape Multibody representa el movimiento lineal de un cubo, útil como punto de partida para simular sistemas mecánicos más complejos. Un ejemplo claro de aplicación sería el modelado de un sistema de suspensión automotriz, donde el movimiento del cubo puede representar el desplazamiento vertical de la rueda o la masa suspendida del vehículo al interactuar con el terreno.

![image](https://github.com/user-attachments/assets/d63b7486-699b-4263-ab57-e3f2b3c9dfc2)

***Fig 18. Suspension***

### Componentes del Modelo

- **Solver Configuration**: Establece los parámetros numéricos necesarios para la resolución del modelo físico.
- **World Frame**: Define el marco de referencia absoluto para todo el sistema mecánico.
- **Mechanism Configuration**: Permite configurar propiedades globales del entorno como la gravedad.
- **Simulink-PS Converter**: Convierte señales de Simulink a señales físicas para interactuar con bloques de Simscape.
- **Rigid Transform**: Ajusta la posición y orientación del sólido respecto al marco global.
- **Brick Solid**: Representa un cubo con propiedades físicas como masa, volumen y geometría.

### Estructura del Sistema

1. **Sistema de Entrada**
   - Una señal de entrada tipo onda (por ejemplo, sinusoidal) es transformada en una señal física mediante el bloque `Simulink-PS Converter`.
   - Esta señal puede representar una fuerza aplicada al sistema.

2. **Transformación Rígida**
   - El bloque `Rigid Transform` define la posición inicial del cubo en el espacio tridimensional.
   - Sirve para conectar correctamente el sólido al marco de referencia global.

3. **Cubo (Brick Solid)**
   - Actúa como el objeto físico que responde a la entrada de fuerza o movimiento.
   - Puede desplazarse linealmente, simulando el comportamiento de una masa móvil.

### Funcionalidad del Sistema

- **Simulación de movimiento lineal**: El sistema está diseñado para analizar el desplazamiento de un sólido (cubo) en una dimensión, producto de una entrada definida.
- **Base para aplicaciones prácticas**: Esta estructura básica puede servir como base para simular fenómenos más complejos, como un sistema de **suspensión automotriz**, en el que el cubo representa la masa suspendida (por ejemplo, el chasis del vehículo).
- **Interacción mecánica**: Permite estudiar la respuesta dinámica del sólido ante una fuerza externa o entrada controlada.

### Simulación

Las **Figuras 19 y 20** muestran el desplazamiento progresivo del cubo a lo largo del eje **X** en distintos momentos de la simulación, como resultado de una entrada física aplicada al sistema.

![image](https://github.com/user-attachments/assets/9e6a6e8f-29c5-4c88-a9dd-a17c4098fe81)

***Fig 19.  Simulación 1 Suspension***

![image](https://github.com/user-attachments/assets/e6d408b4-2446-4b64-be84-d1272e698e41)

***Fig 20.  Simulación 1 Suspension***

---
## 3) Conclusiones

La utilización de bloques como `Solid`, `Prismatic Joint`, y `Simulink-PS Converter` facilita la representación precisa de componentes físicos, así como la interacción entre señales matemáticas y sistemas mecánicos reales.
- La configuración detallada de los parámetros físicos y dinámicos, como masa, geometría y fuerza actuadora, resulta clave para lograr simulaciones realistas y controladas.
- El movimiento oscilatorio vertical prescrito mediante una señal senoidal demostró cómo es posible emular comportamientos dinámicos simples, pero útiles, que pueden escalarse a sistemas más complejos.
- La posibilidad de monitorear variables como la posición mediante sensores permite no solo validar el comportamiento del sistema, sino también diseñar futuros esquemas de control.
- Simscape Multibody se presenta como una herramienta robusta y versátil para el análisis, simulación y validación de mecanismos físicos, ofreciendo un entorno visual e interactivo que mejora el proceso de diseño ingenieril.
- La integración entre Simulink y Simscape Multibody mejora significativamente la visualización de los resultados, permitiendo identificar comportamientos anómalos o inesperados en el sistema mecánico desde una perspectiva gráfica y animada.
- A través de la simulación, es posible modificar rápidamente parámetros como masa, dimensiones o frecuencias, lo cual ahorra tiempo y recursos en la etapa de diseño y verificación.
- La simulación del sistema evidenció la importancia del análisis cinemático y dinámico en el diseño de mecanismos, permitiendo predecir reacciones y desplazamientos ante fuerzas externas o condiciones específicas.
- Se logró fortalecer la capacidad de análisis sistémico del estudiante al requerir la combinación de herramientas de modelado físico con conceptos teóricos de mecánica clásica.
- El proyecto sienta las bases para la implementación de estrategias de control automatizado en sistemas físicos reales, ya que los mismos principios pueden ser aplicados posteriormente a sistemas con actuadores eléctricos, sensores y controladores.

Esta experiencia de modelado ha permitido consolidar los conocimientos sobre la interacción entre los entornos de simulación matemática y física, aportando una base sólida para el diseño y análisis de mecanismos más complejos en el futuro.

## 4) Referencias

- MathWorks. (2024). *Simscape Multibody User’s Guide*. The MathWorks, Inc. https://www.mathworks.com/help/physmod/sm/  
Guía oficial de MathWorks que explica el uso de bloques como Solid, Revolute Joint, Prismatic Joint y Simulink-PS Converter.

- MathWorks. (2024). *Simulink Documentation*. The MathWorks, Inc. https://www.mathworks.com/help/simulink/  
Referencia general del entorno Simulink para la creación y control de modelos mediante señales y algoritmos.

- The MathWorks, Inc. (2020, abril 9). *Physical Modeling Tutorial, Part 6: Introduction to Multibody Simulation* [Video]. YouTube. https://www.youtube.com/watch?v=lItmRlH4iBw  
Tutorial visual sobre modelado físico y simulación multibody con herramientas de MathWorks.

